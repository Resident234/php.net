# debug_print_backtrace





Another way to manipulate and print a backtrace, without using output buffering:



```
<?php
// print backtrace, getting rid of repeated absolute path on each file
$e = new Exception();
print_r(str_replace(&apos;/path/to/code/&apos;, &apos;&apos;, $e-&gt;getTraceAsString()));
?>
```



  

#



I like the output of debug_print_backtrace() but I sometimes want it as a string.



bortuzar&apos;s solution to use output buffering is great, but I&apos;d like to factorize that into a function.&#xA0; Doing that however always results in whatever function name I use appearing at the top of the stack which is redundant.



Below is my noddy (simple) solution.&#xA0; If you don&apos;t care for renumbering the call stack, omit the second preg_replace().





```
<?php

&#xA0; &#xA0; function debug_string_backtrace() {

&#xA0; &#xA0; &#xA0; &#xA0; ob_start();

&#xA0; &#xA0; &#xA0; &#xA0; debug_print_backtrace();

&#xA0; &#xA0; &#xA0; &#xA0; $trace = ob_get_contents();

&#xA0; &#xA0; &#xA0; &#xA0; ob_end_clean();



&#xA0; &#xA0; &#xA0; &#xA0; // Remove first item from backtrace as it&apos;s this function which

&#xA0; &#xA0; &#xA0; &#xA0; // is redundant.

&#xA0; &#xA0; &#xA0; &#xA0; $trace = preg_replace (&apos;/^#0\s+&apos; . __FUNCTION__ . &quot;[^\n]*\n/&quot;, &apos;&apos;, $trace, 1);



&#xA0; &#xA0; &#xA0; &#xA0; // Renumber backtrace items.

&#xA0; &#xA0; &#xA0; &#xA0; $trace = preg_replace (&apos;/^#(\d+)/me&apos;, &apos;\&apos;#\&apos; . ($1 - 1)&apos;, $trace);



&#xA0; &#xA0; &#xA0; &#xA0; return $trace;

&#xA0; &#xA0; }

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.debug-print-backtrace.php)

**[To root](/README.md)**