# Input/output streams



The command line interface data in STDIN is not made available until return is pressed.<br>By adding "readline_callback_handler_install(&apos;&apos;, function(){});" before reading STDIN for the first time single key presses can be captured. <br><br>Note: This only seems to work under Linux CLI and will not work in Apache or Windows CLI.<br><br>This cam be used to obscure a password or used with &apos;stream_select&apos; to make a non blocking keyboard monitor.<br><br>

```
<?php

// Demo WITHOUT readline_callback_handler_install('', function(){});
    $resSTDIN=fopen("php://stdin","r");
    echo("Type 'x'. Then press return.");
    $strChar = stream_get_contents($resSTDIN, 1);

    echo("\nYou typed: ".$strChar."\n\n");
    fclose($resSTDIN);
    
// Demo WITH readline_callback_handler_install('', function(){});
// This line removes the wait for &lt;CR&gt; on STDIN
    readline_callback_handler_install('', function(){});
    
    $resSTDIN=fopen("php://stdin","r");
    echo("We have now run: readline_callback_handler_install('', function(){});\n");
    echo("Press the 'y' key");
    $strChar = stream_get_contents($resSTDIN, 1);
    echo("\nYou pressed: ".$strChar."\nBut did not have to press &lt;cr&gt;\n");
    fclose($resSTDIN);
    readline_callback_handler_remove ();
    echo("\nGoodbye\n")
?>
```


It also hides text from the CLI so can be used for things like. password obscurification. 
eg



```
<?php
    readline_callback_handler_install('', function(){});
    echo("Enter password followed by return. (Do not use a real one!)\n");
    echo("Password: ");
    $strObscured='';
    while(true)
    {
    $strChar = stream_get_contents(STDIN, 1);
    if($strChar===chr(10))
    {
        break;
    }
    $strObscured.=$strChar;
    echo("*");
    }
    echo("\n");
    echo("You entered: ".$strObscured."\n");
?>
```
  

#

Please remember in multi-process applications (which are best suited under CLI), that I/O operations often will BLOCK signals from being processed.<br><br>For instance, if you have a parent waiting on fread(STDIN), it won&apos;t handle SIGCHLD, even if you defined a signal handler for it, until after the call to fread has returned. <br><br>Your solution in this case is to wait on stream_select() to find out whether reading will block. Waiting on stream_select(), critically, does NOT BLOCK signals from being processed. <br><br>Aurelien  

#

The following code shows how to test for input on STDIN.  In this case, we were looking for CSV data, so we use fgetcsv to read STDIN, if it creates an array, we assume CVS input on STDIN, if no array was created, we assume there&apos;s no input from STDIN, and look, later, to an argument with a CSV file name.<br><br>Note, without the stream_set_blocking() call, fgetcsv() hangs on STDIN, awaiting input from the user, which isn&apos;t useful as we&apos;re looking for a piped file. If it isn&apos;t here already, it isn&apos;t going to be.<br><br>

```
<?php
stream_set_blocking(STDIN, 0);
$csv_ar = fgetcsv(STDIN);
if (is_array($csv_ar)){
  print "CVS on STDIN\n";
} else {
  print "Look to ARGV for CSV file name.\n";
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.io-streams.php)

**[To root](/README.md)**