# exit



If you want to avoid calling exit() in FastCGI as per the comments below, but really, positively want to exit cleanly from nested function call or include, consider doing it the Python way:<br><br>define an exception named `SystemExit&apos;, throw it instead of calling exit() and catch it in index.php with an empty handler to finish script execution cleanly.<br><br>

```
<?php

// file: index.php
class SystemExit extends Exception {}
try {
   /* code code */
}
catch (SystemExit $e) { /* do nothing */ }
// end of file: index.php

// some deeply nested function or .php file    

if (SOME_EXIT_CONDITION)
   throw new SystemExit(); // instead of exit()

?>
```
  

#

jbezorg at gmail proposed the following:<br><br>

```
<?php

if($_SERVER[&apos;SCRIPT_FILENAME&apos;] == __FILE__ )
  header(&apos;Location: /&apos;);

?>
```


After sending the `Location:&apos; header PHP _will_ continue parsing, and all code below the header() call will still be executed.  So instead use:



```
<?php

if($_SERVER[&apos;SCRIPT_FILENAME&apos;] == __FILE__)
{
  header(&apos;Location: /&apos;);
  exit;
}

?>
```
  

#

Don&apos;t use the  exit() function in the auto prepend file with fastcgi (linux/bsd os).<br>It has the effect of leaving opened files with for result at least a nice  "Too many open files  ..." error.  

#

To rich dot lovely at klikzltd dot co dot uk:<br><br>Using a "@" before header() to suppress its error, and relying on the "headers already sent" error seems to me a very bad idea while building any serious website.<br><br>This is *not* a clean way to prevent a file from being called directly. At least this is not a secure method, as you rely on the presence of an exception sent by the parser at runtime.<br><br>I recommend using a more common way as defining a constant or assigning a variable with any value, and checking for its presence in the included script, like:<br><br>in index.php:<br>

```
<?php
define (&apos;INDEX&apos;, true);
?>
```


in your included file:


```
<?php
if (!defined(&apos;INDEX&apos;)) {
   die(&apos;You cannot call this script directly !&apos;);
}
?>
```
<br><br>BR.<br><br>Ninj  

#

[Official documentation page](https://www.php.net/manual/en/function.exit.php)

**[To root](/README.md)**