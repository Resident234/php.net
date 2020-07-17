# Error Control Operators



This operator is affectionately known by veteran phpers as the stfu operator.  

#

I was confused as to what the @ symbol actually does, and after a few experiments have concluded the following:<br><br>* the error handler that is set gets called regardless of what level the error reporting is set on, or whether the statement is preceeded with @<br><br>* it is up to the error handler to impart some meaning on the different error levels. You could make your custom error handler echo all errors, even if error reporting is set to NONE.<br><br>* so what does the @ operator do? It temporarily sets the error reporting level to 0 for that line. If that line triggers an error, the error handler will still be called, but it will be called with an error level of 0<br><br>Hope this helps someone  

#

Be aware of using error control operator in statements before include() like this:<br><br>

```
<?php

(@include("file.php"))
 OR die("Could not find file.php!");

?>
```
<br><br>This cause, that error reporting level is set to zero also for the included file. So if there are some errors in the included file, they will be not displayed.  

#

If you&apos;re wondering what the performance impact of using the @ operator is, consider this example.  Here, the second script (using the @ operator) takes 1.75x as long to execute...almost double the time of the first script.<br><br>So while yes, there is some overhead, per iteration, we see that the @ operator added only .005 ms per call.  Not reason enough, imho, to avoid using the @ operator.<br><br>

```
<?php
function x() { }
for ($i = 0; $i < 1000000; $i++) { x(); }
?>
```


real    0m7.617s
user    0m6.788s
sys    0m0.792s

vs



```
<?php
function x() { }
for ($i = 0; $i < 1000000; $i++) { @x(); }
?>
```
<br><br>real    0m13.333s<br>user    0m12.437s<br>sys    0m0.836s  

#

Error suppression should be avoided if possible as it doesn&apos;t just suppress the error that you are trying to stop, but will also suppress errors that you didn&apos;t predict would ever occur. This will make debugging a nightmare.<br><br>It is far better to test for the condition that you know will cause an error before preceding to run the code. This way only the error that you know about will be suppressed and not all future errors associated with that piece of code.<br><br>There may be a good reason for using outright error suppression in favor of the method I have suggested, however in the many years I&apos;ve spent programming web apps I&apos;ve yet to come across a situation where it was a good solution. The examples given on this manual page are certainly not situations where the error control operator should be used.  

#

There is no reason to NOT use something just because "it can be misused".  You could as well say "unlink is evil, you can delete files with it so don&apos;t ever use unlink".<br><br>It&apos;s a valid point that the @ operator hides all errors - so my rule of thumb is: use it only if you&apos;re aware of all possible errors your expression can throw AND you consider all of them irrelevant.<br><br>A simple example is<br>

```
<?php

    $x = @$a["name"];

?>
```
<br>There are only 2 possible problems here: a missing variable or a missing index.  If you&apos;re sure you&apos;re fine with both cases, you&apos;re good to go.  And again: suppressing errors is not a crime.  Not knowing when it&apos;s safe to suppress them is definitely worse.  

#

After some time investigating as to why I was still getting errors that were supposed to be suppressed with @ I found the following.<br><br>1. If you have set your own default error handler then the error still gets sent to the error handler regardless of the @ sign.<br><br>2. As mentioned below the @ suppression only changes the error level for that call. This is not to say that in your error handler you can check the given $errno for a value of 0 as the $errno will still refer to the TYPE(not the error level) of error e.g. E_WARNING or E_ERROR etc<br><br>3. The @ only changes the rumtime error reporting level just for that one call to 0. This means inside your custom error handler you can check the current runtime error_reporting level using error_reporting() (note that one must NOT pass any parameter to this function if you want to get the current value) and if its zero then you know that it has been suppressed.<br>

```
<?php
// Custom error handler
function myErrorHandler($errno, $errstr, $errfile, $errline)
{
    if ( 0 == error_reporting () ) {
        // Error reporting is currently turned off or suppressed with @
        return;
    }
    // Do your normal custom error reporting here
}
?>
```
<br><br>For more info on setting a custom error handler see: http://php.net/manual/en/function.set-error-handler.php<br>For more info on error_reporting see: http://www.php.net/manual/en/function.error-reporting.php  

#

To suppress errors for a new class/object:<br><br>

```
<?php
// Tested: PHP 5.1.2 ~ 2006-10-13

// Typical Example
$var = @some_function();

// Class/Object Example
$var = @new some_class();

// Does NOT Work!
//$var = new @some_class(); // syntax error
?>
```
<br><br>I found this most useful when connecting to a<br>database, where i wanted to control the errors<br>and warnings displayed to the client, while still<br>using the class style of access.  

#

Be aware that using @ is dog-slow, as PHP incurs overhead to suppressing errors in this way. It&apos;s a trade-off between speed and convenience.  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.errorcontrol.php)

**[To root](/README.md)**