# Using PHP from the command line





You can easily parse command line arguments into the $_GET variable by using the parse_str() function.



```
<?php

parse_str(implode(&apos;&amp;&apos;, array_slice($argv, 1)), $_GET);

?>
```


It behaves exactly like you&apos;d expect with cgi-php.

$ php -f somefile.php a=1 b[]=2 b[]=3

This will set $_GET[&apos;a&apos;] to &apos;1&apos; and $_GET[&apos;b&apos;] to array(&apos;2&apos;, &apos;3&apos;).

Even better, instead of putting that line in every file, take advantage of PHP&apos;s auto_prepend_file directive.&#xA0; Put that line in its own file and set the auto_prepend_file directive in your cli-specific php.ini like so:

auto_prepend_file = &quot;/etc/php/cli-php5.3/local.prepend.php&quot;

It will be automatically prepended to any PHP file run from the command line.

  

#



Just a note for people trying to use interactive mode from the commandline.

The purpose of interactive mode is to parse code snippits without actually leaving php, and it works like this:

[root@localhost php-4.3.4]# php -a
Interactive mode enabled



```
<?php echo &quot;hi!&quot;; ?>
```

&lt;note, here we would press CTRL-D to parse everything we&apos;ve entered so far&gt;
hi!


```
<?php exit(); ?>
```

&lt;ctrl-d here again&gt;
[root@localhost php-4.3.4]#

I noticed this somehow got ommited from the docs, hope it helps someone!

  

#



If you want to be interactive with the user and accept user input, all you need to do is read from stdin.&#xA0; 



```
<?php
echo &quot;Are you sure you want to do this?&#xA0; Type &apos;yes&apos; to continue: &quot;;
$handle = fopen (&quot;php://stdin&quot;,&quot;r&quot;);
$line = fgets($handle);
if(trim($line) != &apos;yes&apos;){
&#xA0; &#xA0; echo &quot;ABORTING!\n&quot;;
&#xA0; &#xA0; exit;
}
echo &quot;\n&quot;;
echo &quot;Thank you, continuing...\n&quot;;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.php)

**[To root](/README.md)**