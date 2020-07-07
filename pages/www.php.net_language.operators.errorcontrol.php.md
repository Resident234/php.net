# Error Control Operators





This operator is affectionately known by veteran phpers as the stfu operator.

  

#



I was confused as to what the @ symbol actually does, and after a few experiments have concluded the following:

* the error handler that is set gets called regardless of what level the error reporting is set on, or whether the statement is preceeded with @

* it is up to the error handler to impart some meaning on the different error levels. You could make your custom error handler echo all errors, even if error reporting is set to NONE.

* so what does the @ operator do? It temporarily sets the error reporting level to 0 for that line. If that line triggers an error, the error handler will still be called, but it will be called with an error level of 0

Hope this helps someone

  

#



Be aware of using error control operator in statements before include() like this:

&lt;?PHP

(@include(&quot;file.php&quot;))
 OR die(&quot;Could not find file.php!&quot;);

?>
```


This cause, that error reporting level is set to zero also for the included file. So if there are some errors in the included file, they will be not displayed.

  

#



If you&apos;re wondering what the performance impact of using the @ operator is, consider this example.&#xA0; Here, the second script (using the @ operator) takes 1.75x as long to execute...almost double the time of the first script.



So while yes, there is some overhead, per iteration, we see that the @ operator added only .005 ms per call.&#xA0; Not reason enough, imho, to avoid using the @ operator.





```
<?php

function x() { }

for ($i = 0; $i &lt; 1000000; $i++) { x(); }

?>
```




real&#xA0; &#xA0; 0m7.617s

user&#xA0; &#xA0; 0m6.788s

sys&#xA0; &#xA0; 0m0.792s



vs





```
<?php

function x() { }

for ($i = 0; $i &lt; 1000000; $i++) { @x(); }

?>
```




real&#xA0; &#xA0; 0m13.333s

user&#xA0; &#xA0; 0m12.437s

sys&#xA0; &#xA0; 0m0.836s

  

#



Error suppression should be avoided if possible as it doesn&apos;t just suppress the error that you are trying to stop, but will also suppress errors that you didn&apos;t predict would ever occur. This will make debugging a nightmare.

It is far better to test for the condition that you know will cause an error before preceding to run the code. This way only the error that you know about will be suppressed and not all future errors associated with that piece of code.

There may be a good reason for using outright error suppression in favor of the method I have suggested, however in the many years I&apos;ve spent programming web apps I&apos;ve yet to come across a situation where it was a good solution. The examples given on this manual page are certainly not situations where the error control operator should be used.

  

#



There is no reason to NOT use something just because &quot;it can be misused&quot;.&#xA0; You could as well say &quot;unlink is evil, you can delete files with it so don&apos;t ever use unlink&quot;.

It&apos;s a valid point that the @ operator hides all errors - so my rule of thumb is: use it only if you&apos;re aware of all possible errors your expression can throw AND you consider all of them irrelevant.

A simple example is


```
<?php

&#xA0; &#xA0; $x = @$a[&quot;name&quot;];

?>
```

There are only 2 possible problems here: a missing variable or a missing index.&#xA0; If you&apos;re sure you&apos;re fine with both cases, you&apos;re good to go.&#xA0; And again: suppressing errors is not a crime.&#xA0; Not knowing when it&apos;s safe to suppress them is definitely worse.

  

#



After some time investigating as to why I was still getting errors that were supposed to be suppressed with @ I found the following.

1. If you have set your own default error handler then the error still gets sent to the error handler regardless of the @ sign.

2. As mentioned below the @ suppression only changes the error level for that call. This is not to say that in your error handler you can check the given $errno for a value of 0 as the $errno will still refer to the TYPE(not the error level) of error e.g. E_WARNING or E_ERROR etc

3. The @ only changes the rumtime error reporting level just for that one call to 0. This means inside your custom error handler you can check the current runtime error_reporting level using error_reporting() (note that one must NOT pass any parameter to this function if you want to get the current value) and if its zero then you know that it has been suppressed.


```
<?php
// Custom error handler
function myErrorHandler($errno, $errstr, $errfile, $errline)
{
&#xA0; &#xA0; if ( 0 == error_reporting () ) {
&#xA0; &#xA0; &#xA0; &#xA0; // Error reporting is currently turned off or suppressed with @
&#xA0; &#xA0; &#xA0; &#xA0; return;
&#xA0; &#xA0; }
&#xA0; &#xA0; // Do your normal custom error reporting here
}
?>
```


For more info on setting a custom error handler see: http://php.net/manual/en/function.set-error-handler.php
For more info on error_reporting see: http://www.php.net/manual/en/function.error-reporting.php

  

#



To suppress errors for a new class/object:



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


I found this most useful when connecting to a
database, where i wanted to control the errors
and warnings displayed to the client, while still
using the class style of access.

  

#



Be aware that using @ is dog-slow, as PHP incurs overhead to suppressing errors in this way. It&apos;s a trade-off between speed and convenience.

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.errorcontrol.php)

**[To root](/README.md)**