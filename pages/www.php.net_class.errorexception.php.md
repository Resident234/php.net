# ErrorException





As noted below, it&apos;s important to realize that unless caught, any Exception thrown will halt the script.&#xA0; So converting EVERY notice, warning, or error to an ErrorException will halt your script when something harmlesss like E_USER_NOTICE is triggered.

It seems to me the best use of the ErrorException class is something like this:



```
<?php
function custom_error_handler($number, $string, $file, $line, $context) 
{
&#xA0; &#xA0; // Determine if this error is one of the enabled ones in php config (php.ini, .htaccess, etc)
&#xA0; &#xA0; $error_is_enabled = (bool)($number &amp; ini_get(&apos;error_reporting&apos;) );
&#xA0; &#xA0; 
&#xA0; &#xA0; // -- FATAL ERROR
&#xA0; &#xA0; // throw an Error Exception, to be handled by whatever Exception handling logic is available in this context
&#xA0; &#xA0; if( in_array($number, array(E_USER_ERROR, E_RECOVERABLE_ERROR)) &amp;&amp; $error_is_enabled ) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new ErrorException($errstr, 0, $errno, $errfile, $errline);
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; // -- NON-FATAL ERROR/WARNING/NOTICE
&#xA0; &#xA0; // Log the error if it&apos;s enabled, otherwise just ignore it
&#xA0; &#xA0; else if( $error_is_enabled ) {
&#xA0; &#xA0; &#xA0; &#xA0; error_log( $string, 0 );
&#xA0; &#xA0; &#xA0; &#xA0; return false; // Make sure this ends up in $php_errormsg, if appropriate
&#xA0; &#xA0; }
}
?>
```


Setting this function as the error handler will result in ErrorExceptions only being thrown for E_USER_ERROR and E_RECOVERABLE_ERROR, while other enabled error types will simply get error_log()&apos;ed.

It&apos;s worth noting again that no matter what you do, &quot;E_ERROR, E_PARSE, E_CORE_ERROR, E_CORE_WARNING, E_COMPILE_ERROR, E_COMPILE_WARNING, and most of E_STRICT&quot; will never reach your custom error handler, and therefore will not be converted into ErrorExceptions.&#xA0; Plan accordingly.

  

#



E_USER_WARNING, E_USER_NOTICE, and any other non-terminating error codes, are useless and act like E_USER_ERROR (which terminate) when you combine a custom ERROR_HANDLER with ErrorException and do not CATCH the error. There is NO way to return execution to the parent scope in the EXCEPTION_HANDLER.



```
<?php
&#xA0; &#xA0; 
&#xA0; &#xA0; error_reporting(E_ALL);
&#xA0; &#xA0; define(&apos;DEBUG&apos;, true);
&#xA0; &#xA0; define(&apos;LINEBREAK&apos;, &quot;\r\n&quot;);
&#xA0; &#xA0; 
&#xA0; &#xA0; error::initiate(&apos;./error_backtrace.log&apos;);
&#xA0; &#xA0; 
&#xA0; &#xA0; try
&#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&quot;First error&quot;, E_USER_NOTICE);
&#xA0; &#xA0; catch ( ErrorException $e )
&#xA0; &#xA0; &#xA0; &#xA0; print(&quot;Caught the error: &quot;.$e-&gt;getMessage.&quot;&lt;br /&gt;\r\n&quot; );
&#xA0; &#xA0; 
&#xA0; &#xA0; trigger_error(&quot;This event WILL fire&quot;, E_USER_NOTICE);
&#xA0; &#xA0; 
&#xA0; &#xA0; trigger_error(&quot;This event will NOT fire&quot;, E_USER_NOTICE);
&#xA0; &#xA0; 
&#xA0; &#xA0; abstract class error {
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public static $LIST = array();
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; private function __construct() {}
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public static function initiate( $log = false ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; set_error_handler( &apos;error::err_handler&apos; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; set_exception_handler( &apos;error::exc_handler&apos; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( $log !== false ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( ! ini_get(&apos;log_errors&apos;) )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ini_set(&apos;log_errors&apos;, true);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( ! ini_get(&apos;error_log&apos;) )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ini_set(&apos;error_log&apos;, $log);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public static function err_handler($errno, $errstr, $errfile, $errline, $errcontext) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $l = error_reporting();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( $l &amp; $errno ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $exit = false;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; switch ( $errno ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case E_USER_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $type = &apos;Fatal Error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $exit = true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case E_USER_WARNING:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case E_WARNING:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $type = &apos;Warning&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case E_USER_NOTICE:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case E_NOTICE:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case @E_STRICT:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $type = &apos;Notice&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case @E_RECOVERABLE_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $type = &apos;Catchable&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $type = &apos;Unknown Error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $exit = true;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $exception = new \ErrorException($type.&apos;: &apos;.$errstr, 0, $errno, $errfile, $errline);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( $exit ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; exc_handler($exception);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; exit();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw $exception;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; function exc_handler($exception) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $log = $exception-&gt;getMessage() . &quot;\n&quot; . $exception-&gt;getTraceAsString() . LINEBREAK;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( ini_get(&apos;log_errors&apos;) )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; error_log($log, 0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print(&quot;Unhandled Exception&quot; . (DEBUG ? &quot; - $log&quot; : &apos;&apos;));
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.errorexception.php)

**[To root](/README.md)**