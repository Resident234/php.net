# escapeshellcmd



There is a quirk to be aware of regarding use of echo. If you have a command which you want to execute which takes input from STDIN, you would normally do:<br><br>

```
<?php $output = shell_exec("echo $input | /the/command"); ?>
```


Unfortunately, this is a *bad idea* and will make your script unportable, providing a very hard-to-trace bug on some systems. Depending on how the server is set up, /bin/sh will either call /bin/bash or /bin/dash, and these have very different versions of echo. Never use echo; use printf instead which is consistent. How do you escape for printf? Do this:



```
<?php
$input = &apos;string to be passed *exactly* to the command&apos;;
//Escape only what is needed to get by PHP&apos;s parser; we want
//the string data PHP is holding in its buffer to be passed
//exactly to stdin buffer of the command.
$cmd = str_replace(array(&apos;\\&apos;, &apos;%&apos;), array(&apos;\\\\&apos;, &apos;%%&apos;), $input);
$cmd = escapeshellarg($cmd);

$output = shell_exec("printf $cmd | /path/to/command");
?>
```


For the paranoid, this torture test verifies that both shell escaping and printf&apos;s own escaping are handled correctly. Use with confidence!



```
<?php

$test = &apos;stuff bash interprets, space: # &amp; ; ` | * ? ~ &lt; &gt; ^ ( ) [ ] { } $ \ \x0A \xFF. \&apos; " %&apos;.PHP_EOL.
        &apos;stuff bash interprets, no space: #&amp;;`|*?~&lt;&gt;^()[]{}$\\x0A\xFF.\&apos;\"%&apos;.PHP_EOL.
        &apos;stuff bash interprets, with leading backslash: \# \&amp; \; \` \| \* \? \~ \&lt; \&gt; \^ \( \) \[ \] \{ \} \$ \\\ \\\x0A \\\xFF. \\\&apos; \" \%&apos;.PHP_EOL.
        &apos;printf codes: % \ (used to form %.0#-*+d, or \\ \a \b \f \n \r \t \v \" \? \062 \0062 \x032 \u0032 and \U00000032)&apos;;

echo "These are the strings we are testing with:".PHP_EOL.$test.PHP_EOL;
$cmd = $test;
$cmd = str_replace(array(&apos;\\&apos;, &apos;%&apos;), array(&apos;\\\\&apos;, &apos;%%&apos;), $test);
$cmd = escapeshellarg($cmd);

echo PHP_EOL."This is the output using the escaping mechanism given:".PHP_EOL;
echo `printf $cmd | cat`.PHP_EOL;

echo PHP_EOL."They should match exactly".PHP_EOL;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.escapeshellcmd.php)

**[To root](/README.md)**