# trigger_error



the idea is never to give out file names, line numbers, and cryptic codes to the user. Use trigger_error() after you used set_error_handler() to register your own callback function which either logs or emails the error codes to you, and echo a simple friendly message to the user.<br><br>And turn on a more verbose error handler function when you need to debug your scripts. In my init.php scripts I always have:<br><br>if (_DEBUG_) {<br>    set_error_handler (&apos;debug_error_handler&apos;);<br>}<br>else {<br>    set_error_handler (&apos;nice_error_handler&apos;);<br>}  

#

trigger_error always reports the line and file that trigger_error was called on. Which isn&apos;t very useful.<br><br>eg:<br><br>main.php:<br>

```
<?php
include(&apos;functions.php&apos;);
$x = &apos;test&apos;;
doFunction($x);
?>
```


functions.php:


```
<?php
function doFunction($var) {
if(is_numeric($var)) {
 /* do some stuff*/
} else {
 trigger_error(&apos;var must be numeric&apos;);
}
}
?>
```


will output "Notice: var must be numeric in functions.php on line 6"
whereas "Notice: var must be numeric in main.php on line 4" would be more useful

here&apos;s a function to do that:



```
<?php

function error($message, $level=E_USER_NOTICE) {
$caller = next(debug_backtrace());
trigger_error($message.&apos; in &lt;strong&gt;&apos;.$caller[&apos;function&apos;].&apos;&lt;/strong&gt; called from &lt;strong&gt;&apos;.$caller[&apos;file&apos;].&apos;&lt;/strong&gt; on line &lt;strong&gt;&apos;.$caller[&apos;line&apos;].&apos;&lt;/strong&gt;&apos;."\n&lt;br /&gt;error handler", $level);
}
?>
```


So now in our example:

main.php:


```
<?php
include(&apos;functions.php&apos;);
$x = &apos;test&apos;;
doFunction($x);
?>
```


functions.php:


```
<?php
function doFunction($var) {
    if(is_numeric($var)) {
         /* do some stuff*/
    } else {
         error(&apos;var must be numeric&apos;);
    }
}

function error($message, $level=E_USER_NOTICE) {
    $caller = next(debug_backtrace());
    trigger_error($message.&apos; in &lt;strong&gt;&apos;.$caller[&apos;function&apos;].&apos;&lt;/strong&gt; called from &lt;strong&gt;&apos;.$caller[&apos;file&apos;].&apos;&lt;/strong&gt; on line &lt;strong&gt;&apos;.$caller[&apos;line&apos;].&apos;&lt;/strong&gt;&apos;."\n&lt;br /&gt;error handler", $level);
}
?>
```
<br><br>now outputs:<br><br>"Notice: var must be numeric in doFunction called from main.php on line 4"  

#

[Official documentation page](https://www.php.net/manual/en/function.trigger-error.php)

**[To root](/README.md)**