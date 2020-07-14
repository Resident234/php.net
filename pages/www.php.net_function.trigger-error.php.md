# trigger_error



the idea is never to give out file names, line numbers, and cryptic codes to the user. Use trigger_error() after you used set_error_handler() to register your own callback function which either logs or emails the error codes to you, and echo a simple friendly message to the user.<br><br>And turn on a more verbose error handler function when you need to debug your scripts. In my init.php scripts I always have:<br><br>if (_DEBUG_) {<br>    set_error_handler (&apos;debug_error_handler&apos;);<br>}<br>else {<br>    set_error_handler (&apos;nice_error_handler&apos;);<br>}  

#

trigger_error always reports the line and file that trigger_error was called on. Which isn&apos;t very useful.<br><br>eg:<br><br>main.php:<br>

```
<?php
include('functions.php');
$x = 'test';
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
 trigger_error('var must be numeric');
}
}
?>
```


will output "Notice: var must be numeric in functions.php on line 6"
whereas "Notice: var must be numeric in main.php on line 4" would be more useful

here's a function to do that:



```
<?php

function error($message, $level=E_USER_NOTICE) {
$caller = next(debug_backtrace());
trigger_error($message.' in &lt;strong&gt;'.$caller['function'].'&lt;/strong&gt; called from &lt;strong&gt;'.$caller['file'].'&lt;/strong&gt; on line &lt;strong&gt;'.$caller['line'].'&lt;/strong&gt;'."\n&lt;br /&gt;error handler", $level);
}
?>
```


So now in our example:

main.php:


```
<?php
include('functions.php');
$x = 'test';
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
         error('var must be numeric');
    }
}

function error($message, $level=E_USER_NOTICE) {
    $caller = next(debug_backtrace());
    trigger_error($message.' in &lt;strong&gt;'.$caller['function'].'&lt;/strong&gt; called from &lt;strong&gt;'.$caller['file'].'&lt;/strong&gt; on line &lt;strong&gt;'.$caller['line'].'&lt;/strong&gt;'."\n&lt;br /&gt;error handler", $level);
}
?>
```
<br><br>now outputs:<br><br>"Notice: var must be numeric in doFunction called from main.php on line 4"  

#

[Official documentation page](https://www.php.net/manual/en/function.trigger-error.php)

**[To root](/README.md)**