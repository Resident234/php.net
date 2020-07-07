# set_error_handler





By this function alone you can not catch fatal errors, there is a simple work around. Below is part of my error.php file which handles errors and exceptions in the application. Before someone complains I&apos;ll add that I do not care that I am using globals, this file is part of my mini framework and without the &apos;config&apos; variable the application would crash anyways.



```
<?php

/**
 * Error handler, passes flow over the exception logger with new ErrorException.
 */
function log_error( $num, $str, $file, $line, $context = null )
{
&#xA0; &#xA0; log_exception( new ErrorException( $str, 0, $num, $file, $line ) );
}

/**
 * Uncaught exception handler.
 */
function log_exception( Exception $e )
{
&#xA0; &#xA0; global $config;
&#xA0; &#xA0; 
&#xA0; &#xA0; if ( $config[&quot;debug&quot;] == true )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;div style=&apos;text-align: center;&apos;&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;h2 style=&apos;color: rgb(190, 50, 50);&apos;&gt;Exception Occured:&lt;/h2&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;table style=&apos;width: 800px; display: inline-block;&apos;&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;tr style=&apos;background-color:rgb(230,230,230);&apos;&gt;&lt;th style=&apos;width: 80px;&apos;&gt;Type&lt;/th&gt;&lt;td&gt;&quot; . get_class( $e ) . &quot;&lt;/td&gt;&lt;/tr&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;tr style=&apos;background-color:rgb(240,240,240);&apos;&gt;&lt;th&gt;Message&lt;/th&gt;&lt;td&gt;{$e-&gt;getMessage()}&lt;/td&gt;&lt;/tr&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;tr style=&apos;background-color:rgb(230,230,230);&apos;&gt;&lt;th&gt;File&lt;/th&gt;&lt;td&gt;{$e-&gt;getFile()}&lt;/td&gt;&lt;/tr&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;tr style=&apos;background-color:rgb(240,240,240);&apos;&gt;&lt;th&gt;Line&lt;/th&gt;&lt;td&gt;{$e-&gt;getLine()}&lt;/td&gt;&lt;/tr&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;&lt;/table&gt;&lt;/div&gt;&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; else
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $message = &quot;Type: &quot; . get_class( $e ) . &quot;; Message: {$e-&gt;getMessage()}; File: {$e-&gt;getFile()}; Line: {$e-&gt;getLine()};&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; file_put_contents( $config[&quot;app_dir&quot;] . &quot;/tmp/logs/exceptions.log&quot;, $message . PHP_EOL, FILE_APPEND );
&#xA0; &#xA0; &#xA0; &#xA0; header( &quot;Location: {$config[&quot;error_page&quot;]}&quot; );
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; exit();
}

/**
 * Checks for a fatal error, work around for set_error_handler not working on fatal errors.
 */
function check_for_fatal()
{
&#xA0; &#xA0; $error = error_get_last();
&#xA0; &#xA0; if ( $error[&quot;type&quot;] == E_ERROR )
&#xA0; &#xA0; &#xA0; &#xA0; log_error( $error[&quot;type&quot;], $error[&quot;message&quot;], $error[&quot;file&quot;], $error[&quot;line&quot;] );
}

register_shutdown_function( &quot;check_for_fatal&quot; );
set_error_handler( &quot;log_error&quot; );
set_exception_handler( &quot;log_exception&quot; );
ini_set( &quot;display_errors&quot;, &quot;off&quot; );
error_reporting( E_ALL );


  

#





```
<?php
/**
 * throw exceptions based on E_* error types
 */
set_error_handler(function ($err_severity, $err_msg, $err_file, $err_line, array $err_context)
{
&#xA0; &#xA0; // error was suppressed with the @-operator
&#xA0; &#xA0; if (0 === error_reporting()) { return false;}
&#xA0; &#xA0; switch($err_severity)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; case E_ERROR:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new ErrorException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_WARNING:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new WarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_PARSE:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new ParseException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_NOTICE:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new NoticeException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_CORE_ERROR:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new CoreErrorException&#xA0; &#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_CORE_WARNING:&#xA0; &#xA0; &#xA0; &#xA0; throw new CoreWarningException&#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_COMPILE_ERROR:&#xA0; &#xA0; &#xA0;&#xA0; throw new CompileErrorException&#xA0; &#xA0;&#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_COMPILE_WARNING:&#xA0; &#xA0;&#xA0; throw new CoreWarningException&#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_ERROR:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new UserErrorException&#xA0; &#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_WARNING:&#xA0; &#xA0; &#xA0; &#xA0; throw new UserWarningException&#xA0; &#xA0; &#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_NOTICE:&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; throw new UserNoticeException&#xA0; &#xA0; &#xA0;&#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_STRICT:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new StrictException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_RECOVERABLE_ERROR:&#xA0;&#xA0; throw new RecoverableErrorException ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_DEPRECATED:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new DeprecatedException&#xA0; &#xA0; &#xA0;&#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_DEPRECATED:&#xA0; &#xA0;&#xA0; throw new UserDeprecatedException&#xA0;&#xA0; ($err_msg, 0, $err_severity, $err_file, $err_line);
&#xA0; &#xA0; }
});

class WarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; extends ErrorException {}
class ParseException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; extends ErrorException {}
class NoticeException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}
class CoreErrorException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; extends ErrorException {}
class CoreWarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; extends ErrorException {}
class CompileErrorException&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}
class CompileWarningException&#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}
class UserErrorException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; extends ErrorException {}
class UserWarningException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; extends ErrorException {}
class UserNoticeException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}
class StrictException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}
class RecoverableErrorException&#xA0; &#xA0;&#xA0; extends ErrorException {}
class DeprecatedException&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}
class UserDeprecatedException&#xA0; &#xA0; &#xA0;&#xA0; extends ErrorException {}


  

#





```
<?php
/**
 * Used for logging all php notices,warings and etc in a file when error reporting
 * is set and display_errors is off
 * @uses used in prod env for logging all type of error of php code in a file for further debugging
 * and code performance
 * @author Aditya Mehrotra&lt;aditycse@gmail.com&gt;
 */
error_reporting(E_ALL);
ini_set(&quot;display_errors&quot;, &quot;off&quot;);
define(&apos;ERROR_LOG_FILE&apos;, &apos;/var/www/error.log&apos;);

/**
 * Custom error handler
 * @param integer $code
 * @param string $description
 * @param string $file
 * @param interger $line
 * @param mixed $context
 * @return boolean
 */
function handleError($code, $description, $file = null, $line = null, $context = null) {
&#xA0; &#xA0; $displayErrors = ini_get(&quot;display_errors&quot;);
&#xA0; &#xA0; $displayErrors = strtolower($displayErrors);
&#xA0; &#xA0; if (error_reporting() === 0 || $displayErrors === &quot;on&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; }
&#xA0; &#xA0; list($error, $log) = mapErrorCode($code);
&#xA0; &#xA0; $data = array(
&#xA0; &#xA0; &#xA0; &#xA0; &apos;level&apos; =&gt; $log,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;code&apos; =&gt; $code,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;error&apos; =&gt; $error,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;description&apos; =&gt; $description,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;file&apos; =&gt; $file,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;line&apos; =&gt; $line,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;context&apos; =&gt; $context,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;path&apos; =&gt; $file,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;message&apos; =&gt; $error . &apos; (&apos; . $code . &apos;): &apos; . $description . &apos; in [&apos; . $file . &apos;, line &apos; . $line . &apos;]&apos;
&#xA0; &#xA0; );
&#xA0; &#xA0; return fileLog($data);
}

/**
 * This method is used to write data in file
 * @param mixed $logData
 * @param string $fileName
 * @return boolean
 */
function fileLog($logData, $fileName = ERROR_LOG_FILE) {
&#xA0; &#xA0; $fh = fopen($fileName, &apos;a+&apos;);
&#xA0; &#xA0; if (is_array($logData)) {
&#xA0; &#xA0; &#xA0; &#xA0; $logData = print_r($logData, 1);
&#xA0; &#xA0; }
&#xA0; &#xA0; $status = fwrite($fh, $logData);
&#xA0; &#xA0; fclose($fh);
&#xA0; &#xA0; return ($status) ? true : false;
}

/**
 * Map an error code into an Error word, and log location.
 *
 * @param int $code Error code to map
 * @return array Array of error word, and log location.
 */
function mapErrorCode($code) {
&#xA0; &#xA0; $error = $log = null;
&#xA0; &#xA0; switch ($code) {
&#xA0; &#xA0; &#xA0; &#xA0; case E_PARSE:
&#xA0; &#xA0; &#xA0; &#xA0; case E_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; case E_CORE_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; case E_COMPILE_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = &apos;Fatal Error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $log = LOG_ERR;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case E_WARNING:
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_WARNING:
&#xA0; &#xA0; &#xA0; &#xA0; case E_COMPILE_WARNING:
&#xA0; &#xA0; &#xA0; &#xA0; case E_RECOVERABLE_ERROR:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = &apos;Warning&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $log = LOG_WARNING;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case E_NOTICE:
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_NOTICE:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = &apos;Notice&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $log = LOG_NOTICE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case E_STRICT:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = &apos;Strict&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $log = LOG_NOTICE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case E_DEPRECATED:
&#xA0; &#xA0; &#xA0; &#xA0; case E_USER_DEPRECATED:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $error = &apos;Deprecated&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $log = LOG_NOTICE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; default :
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }
&#xA0; &#xA0; return array($error, $log);
}

//calling custom error handler
set_error_handler(&quot;handleError&quot;);

print_r($arra); //undefined variable
print_r($dssdfdfgg); //undefined variable
include_once &apos;file.php&apos;; //No such file or directory
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.set-error-handler.php)

**[To root](/README.md)**