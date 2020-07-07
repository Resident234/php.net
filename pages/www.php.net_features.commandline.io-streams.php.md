# Input/output streams





The command line interface data in STDIN is not made available until return is pressed.
By adding &quot;readline_callback_handler_install(&apos;&apos;, function(){});&quot; before reading STDIN for the first time single key presses can be captured. 

Note: This only seems to work under Linux CLI and will not work in Apache or Windows CLI.

This cam be used to obscure a password or used with &apos;stream_select&apos; to make a non blocking keyboard monitor.



```
<?php

// Demo WITHOUT readline_callback_handler_install(&apos;&apos;, function(){});
&#xA0; &#xA0; $resSTDIN=fopen(&quot;php://stdin&quot;,&quot;r&quot;);
&#xA0; &#xA0; echo(&quot;Type &apos;x&apos;. Then press return.&quot;);
&#xA0; &#xA0; $strChar = stream_get_contents($resSTDIN, 1);

&#xA0; &#xA0; echo(&quot;\nYou typed: &quot;.$strChar.&quot;\n\n&quot;);
&#xA0; &#xA0; fclose($resSTDIN);
&#xA0; &#xA0; 
// Demo WITH readline_callback_handler_install(&apos;&apos;, function(){});
// This line removes the wait for &lt;CR&gt; on STDIN
&#xA0; &#xA0; readline_callback_handler_install(&apos;&apos;, function(){});
&#xA0; &#xA0; 
&#xA0; &#xA0; $resSTDIN=fopen(&quot;php://stdin&quot;,&quot;r&quot;);
&#xA0; &#xA0; echo(&quot;We have now run: readline_callback_handler_install(&apos;&apos;, function(){});\n&quot;);
&#xA0; &#xA0; echo(&quot;Press the &apos;y&apos; key&quot;);
&#xA0; &#xA0; $strChar = stream_get_contents($resSTDIN, 1);
&#xA0; &#xA0; echo(&quot;\nYou pressed: &quot;.$strChar.&quot;\nBut did not have to press &lt;cr&gt;\n&quot;);
&#xA0; &#xA0; fclose($resSTDIN);
&#xA0; &#xA0; readline_callback_handler_remove ();
&#xA0; &#xA0; echo(&quot;\nGoodbye\n&quot;)
?>
```


It also hides text from the CLI so can be used for things like. password obscurification. 
eg



```
<?php
&#xA0; &#xA0; readline_callback_handler_install(&apos;&apos;, function(){});
&#xA0; &#xA0; echo(&quot;Enter password followed by return. (Do not use a real one!)\n&quot;);
&#xA0; &#xA0; echo(&quot;Password: &quot;);
&#xA0; &#xA0; $strObscured=&apos;&apos;;
&#xA0; &#xA0; while(true)
&#xA0; &#xA0; {
&#xA0; &#xA0; $strChar = stream_get_contents(STDIN, 1);
&#xA0; &#xA0; if($strChar===chr(10))
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }
&#xA0; &#xA0; $strObscured.=$strChar;
&#xA0; &#xA0; echo(&quot;*&quot;);
&#xA0; &#xA0; }
&#xA0; &#xA0; echo(&quot;\n&quot;);
&#xA0; &#xA0; echo(&quot;You entered: &quot;.$strObscured.&quot;\n&quot;);
?>
```



  

#



Please remember in multi-process applications (which are best suited under CLI), that I/O operations often will BLOCK signals from being processed.

For instance, if you have a parent waiting on fread(STDIN), it won&apos;t handle SIGCHLD, even if you defined a signal handler for it, until after the call to fread has returned. 

Your solution in this case is to wait on stream_select() to find out whether reading will block. Waiting on stream_select(), critically, does NOT BLOCK signals from being processed. 

Aurelien

  

#



The following code shows how to test for input on STDIN.&#xA0; In this case, we were looking for CSV data, so we use fgetcsv to read STDIN, if it creates an array, we assume CVS input on STDIN, if no array was created, we assume there&apos;s no input from STDIN, and look, later, to an argument with a CSV file name.

Note, without the stream_set_blocking() call, fgetcsv() hangs on STDIN, awaiting input from the user, which isn&apos;t useful as we&apos;re looking for a piped file. If it isn&apos;t here already, it isn&apos;t going to be.



```
<?php
stream_set_blocking(STDIN, 0);
$csv_ar = fgetcsv(STDIN);
if (is_array($csv_ar)){
&#xA0; print &quot;CVS on STDIN\n&quot;;
} else {
&#xA0; print &quot;Look to ARGV for CSV file name.\n&quot;;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.io-streams.php)

**[To root](/README.md)**