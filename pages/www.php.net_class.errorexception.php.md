# ErrorException



As noted below, it&apos;s important to realize that unless caught, any Exception thrown will halt the script.  So converting EVERY notice, warning, or error to an ErrorException will halt your script when something harmlesss like E_USER_NOTICE is triggered.<br><br>It seems to me the best use of the ErrorException class is something like this:<br><br>

```
<?php
function custom_error_handler($number, $string, $file, $line, $context) 
{
    // Determine if this error is one of the enabled ones in php config (php.ini, .htaccess, etc)
    $error_is_enabled = (bool)($number &amp; ini_get(&apos;error_reporting&apos;) );
    
    // -- FATAL ERROR
    // throw an Error Exception, to be handled by whatever Exception handling logic is available in this context
    if( in_array($number, array(E_USER_ERROR, E_RECOVERABLE_ERROR)) &amp;&amp; $error_is_enabled ) {
        throw new ErrorException($errstr, 0, $errno, $errfile, $errline);
    }
    
    // -- NON-FATAL ERROR/WARNING/NOTICE
    // Log the error if it&apos;s enabled, otherwise just ignore it
    else if( $error_is_enabled ) {
        error_log( $string, 0 );
        return false; // Make sure this ends up in $php_errormsg, if appropriate
    }
}
?>
```
<br><br>Setting this function as the error handler will result in ErrorExceptions only being thrown for E_USER_ERROR and E_RECOVERABLE_ERROR, while other enabled error types will simply get error_log()&apos;ed.<br><br>It&apos;s worth noting again that no matter what you do, "E_ERROR, E_PARSE, E_CORE_ERROR, E_CORE_WARNING, E_COMPILE_ERROR, E_COMPILE_WARNING, and most of E_STRICT" will never reach your custom error handler, and therefore will not be converted into ErrorExceptions.  Plan accordingly.  

#

E_USER_WARNING, E_USER_NOTICE, and any other non-terminating error codes, are useless and act like E_USER_ERROR (which terminate) when you combine a custom ERROR_HANDLER with ErrorException and do not CATCH the error. There is NO way to return execution to the parent scope in the EXCEPTION_HANDLER.<br><br>

```
<?php
    
    error_reporting(E_ALL);
    define(&apos;DEBUG&apos;, true);
    define(&apos;LINEBREAK&apos;, "\r\n");
    
    error::initiate(&apos;./error_backtrace.log&apos;);
    
    try
        trigger_error("First error", E_USER_NOTICE);
    catch ( ErrorException $e )
        print("Caught the error: ".$e-&gt;getMessage."&lt;br /&gt;\r\n" );
    
    trigger_error("This event WILL fire", E_USER_NOTICE);
    
    trigger_error("This event will NOT fire", E_USER_NOTICE);
    
    abstract class error {
        
        public static $LIST = array();
        
        private function __construct() {}
        
        public static function initiate( $log = false ) {
            set_error_handler( &apos;error::err_handler&apos; );
            set_exception_handler( &apos;error::exc_handler&apos; );
            if ( $log !== false ) {
                if ( ! ini_get(&apos;log_errors&apos;) )
                    ini_set(&apos;log_errors&apos;, true);
                if ( ! ini_get(&apos;error_log&apos;) )
                    ini_set(&apos;error_log&apos;, $log);
            }
        }
        
        public static function err_handler($errno, $errstr, $errfile, $errline, $errcontext) {
            $l = error_reporting();
            if ( $l &amp; $errno ) {
                
                $exit = false;
                switch ( $errno ) {
                    case E_USER_ERROR:
                        $type = &apos;Fatal Error&apos;;
                        $exit = true;
                    break;
                    case E_USER_WARNING:
                    case E_WARNING:
                        $type = &apos;Warning&apos;;
                    break;
                    case E_USER_NOTICE:
                    case E_NOTICE:
                    case @E_STRICT:
                        $type = &apos;Notice&apos;;
                    break;
                    case @E_RECOVERABLE_ERROR:
                        $type = &apos;Catchable&apos;;
                    break;
                    default:
                        $type = &apos;Unknown Error&apos;;
                        $exit = true;
                    break;
                }
                
                $exception = new \ErrorException($type.&apos;: &apos;.$errstr, 0, $errno, $errfile, $errline);
                
                if ( $exit ) {
                    exc_handler($exception);
                    exit();
                }
                else
                    throw $exception;
            }
            return false;
        }
        
        function exc_handler($exception) {
            $log = $exception-&gt;getMessage() . "\n" . $exception-&gt;getTraceAsString() . LINEBREAK;
            if ( ini_get(&apos;log_errors&apos;) )
                error_log($log, 0);
            print("Unhandled Exception" . (DEBUG ? " - $log" : &apos;&apos;));
        }
        
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.errorexception.php)

**[To root](/README.md)**