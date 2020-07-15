# Using PHP from the command line



You can easily parse command line arguments into the $_GET variable by using the parse_str() function.<br><br>

```
<?php

parse_str(implode('&amp;', array_slice($argv, 1)), $_GET);

?>
```
<br><br>It behaves exactly like you&apos;d expect with cgi

```
<?php.<br><br>$ php -f somefile.php a=1 b[]=2 b[]=3<br><br>This will set $_GET[&apos;a&apos;] to &apos;1&apos; and $_GET[&apos;b&apos;] to array(&apos;2&apos;, &apos;3&apos;).<br><br>Even better, instead of putting that line in every file, take advantage of PHP&apos;s auto_prepend_file directive.  Put that line in its own file and set the auto_prepend_file directive in your cli-specific php.ini like so:<br><br>auto_prepend_file = "/etc/php/cli

```
<?php5.3/local.prepend.php"<br><br>It will be automatically prepended to any PHP file run from the command line.  

#

Just a note for people trying to use interactive mode from the commandline.<br><br>The purpose of interactive mode is to parse code snippits without actually leaving php, and it works like this:<br><br>[root@localhost ?>
```
4.3.4]# php -a<br>Interactive mode enabled<br><br>

```
<?php echo "hi!"; ?>
```

<note, here we would press CTRL-D to parse everything we've entered so far>
hi!


```
<?php exit(); ?>
```

<ctrl-d here again>
[root@localhost ?>
```
4.3.4]#<br><br>I noticed this somehow got ommited from the docs, hope it helps someone!  

#

If you want to be interactive with the user and accept user input, all you need to do is read from stdin.  <br><br>

```
<?php
echo "Are you sure you want to do this?  Type 'yes' to continue: ";
$handle = fopen ("php://stdin","r");
$line = fgets($handle);
if(trim($line) != 'yes'){
    echo "ABORTING!\n";
    exit;
}
echo "\n";
echo "Thank you, continuing...\n";
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.php)

**[To root](/README.md)**