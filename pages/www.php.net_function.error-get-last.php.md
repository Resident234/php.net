# error_get_last





[Editor&apos;s note: as of PHP 7.0.0 there is error_clear_last() to clear the most recent error.]



To clear error_get_last(), or put it in a well defined state, you should use the code below. It works even when a custom error handler has been set.





```
<?php



// var_dump or anything else, as this will never be called because of the 0

set_error_handler(&apos;var_dump&apos;, 0);

@$undef_var;

restore_error_handler();



// error_get_last() is now in a well known state:

// Undefined variable: undef_var



... // Do something



$e = error_get_last();



...



?>
```



  

#



If an error handler (see set_error_handler ) successfully handles an error then that error will not be reported by this function.

  

#



The error_get_last() function will give you the most recent error even when that error is a Fatal error.

Example Usage:



```
<?php

register_shutdown_function(&apos;handleFatalPhpError&apos;);

function handleFatalPhpError() {
&#xA0;&#xA0; $last_error = error_get_last();
&#xA0;&#xA0; if($last_error[&apos;type&apos;] === E_ERROR) {
&#xA0; &#xA0; &#xA0; echo &quot;Can do custom output and/or logging for fatal error here...&quot;;
&#xA0;&#xA0; }
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.error-get-last.php)

**[To root](/README.md)**