# set_error_handler



By this function alone you can not catch fatal errors, there is a simple work around. Below is part of my error.php file which handles errors and exceptions in the application. Before someone complains I&apos;ll add that I do not care that I am using globals, this file is part of my mini framework and without the &apos;config&apos; variable the application would crash anyways.<br><br>

```
<?php<br><br>/**<br> * Error handler, passes flow over the exception logger with new ErrorException.<br> */<br>function log_error( $num, $str, $file, $line, $context = null )<br>{<br>    log_exception( new ErrorException( $str, 0, $num, $file, $line ) );<br>}<br><br>/**<br> * Uncaught exception handler.<br> */<br>function log_exception( Exception $e )<br>{<br>    global $config;<br>    <br>    if ( $config["debug"] == true )<br>    {<br>        print "&lt;div style=&apos;text-align: center;&apos;&gt;";<br>        print "&lt;h2 style=&apos;color: rgb(190, 50, 50);&apos;&gt;Exception Occured:&lt;/h2&gt;";<br>        print "&lt;table style=&apos;width: 800px; display: inline-block;&apos;&gt;";<br>        print "&lt;tr style=&apos;background-color:rgb(230,230,230);&apos;&gt;&lt;th style=&apos;width: 80px;&apos;&gt;Type&lt;/th&gt;&lt;td&gt;" . get_class( $e ) . "&lt;/td&gt;&lt;/tr&gt;";<br>        print "&lt;tr style=&apos;background-color:rgb(240,240,240);&apos;&gt;&lt;th&gt;Message&lt;/th&gt;&lt;td&gt;{$e-&gt;getMessage()}&lt;/td&gt;&lt;/tr&gt;";<br>        print "&lt;tr style=&apos;background-color:rgb(230,230,230);&apos;&gt;&lt;th&gt;File&lt;/th&gt;&lt;td&gt;{$e-&gt;getFile()}&lt;/td&gt;&lt;/tr&gt;";<br>        print "&lt;tr style=&apos;background-color:rgb(240,240,240);&apos;&gt;&lt;th&gt;Line&lt;/th&gt;&lt;td&gt;{$e-&gt;getLine()}&lt;/td&gt;&lt;/tr&gt;";<br>        print "&lt;/table&gt;&lt;/div&gt;";<br>    }<br>    else<br>    {<br>        $message = "Type: " . get_class( $e ) . "; Message: {$e-&gt;getMessage()}; File: {$e-&gt;getFile()}; Line: {$e-&gt;getLine()};";<br>        file_put_contents( $config["app_dir"] . "/tmp/logs/exceptions.log", $message . PHP_EOL, FILE_APPEND );<br>        header( "Location: {$config["error_page"]}" );<br>    }<br>    <br>    exit();<br>}<br><br>/**<br> * Checks for a fatal error, work around for set_error_handler not working on fatal errors.<br> */<br>function check_for_fatal()<br>{<br>    $error = error_get_last();<br>    if ( $error["type"] == E_ERROR )<br>        log_error( $error["type"], $error["message"], $error["file"], $error["line"] );<br>}<br><br>register_shutdown_function( "check_for_fatal" );<br>set_error_handler( "log_error" );<br>set_exception_handler( "log_exception" );<br>ini_set( "display_errors", "off" );<br>error_reporting( E_ALL );  

#



```
<?php<br>/**<br> * throw exceptions based on E_* error types<br> */<br>set_error_handler(function ($err_severity, $err_msg, $err_file, $err_line, array $err_context)<br>{<br>    // error was suppressed with the @-operator<br>    if (0 === error_reporting()) { return false;}<br>    switch($err_severity)<br>    {<br>        case E_ERROR:               throw new ErrorException            ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_WARNING:             throw new WarningException          ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_PARSE:               throw new ParseException            ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_NOTICE:              throw new NoticeException           ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_CORE_ERROR:          throw new CoreErrorException        ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_CORE_WARNING:        throw new CoreWarningException      ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_COMPILE_ERROR:       throw new CompileErrorException     ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_COMPILE_WARNING:     throw new CoreWarningException      ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_USER_ERROR:          throw new UserErrorException        ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_USER_WARNING:        throw new UserWarningException      ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_USER_NOTICE:         throw new UserNoticeException       ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_STRICT:              throw new StrictException           ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_RECOVERABLE_ERROR:   throw new RecoverableErrorException ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_DEPRECATED:          throw new DeprecatedException       ($err_msg, 0, $err_severity, $err_file, $err_line);<br>        case E_USER_DEPRECATED:     throw new UserDeprecatedException   ($err_msg, 0, $err_severity, $err_file, $err_line);<br>    }<br>});<br><br>class WarningException              extends ErrorException {}<br>class ParseException                extends ErrorException {}<br>class NoticeException               extends ErrorException {}<br>class CoreErrorException            extends ErrorException {}<br>class CoreWarningException          extends ErrorException {}<br>class CompileErrorException         extends ErrorException {}<br>class CompileWarningException       extends ErrorException {}<br>class UserErrorException            extends ErrorException {}<br>class UserWarningException          extends ErrorException {}<br>class UserNoticeException           extends ErrorException {}<br>class StrictException               extends ErrorException {}<br>class RecoverableErrorException     extends ErrorException {}<br>class DeprecatedException           extends ErrorException {}<br>class UserDeprecatedException       extends ErrorException {}  

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
ini_set("display_errors", "off");
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
    $displayErrors = ini_get("display_errors");
    $displayErrors = strtolower($displayErrors);
    if (error_reporting() === 0 || $displayErrors === "on") {
        return false;
    }
    list($error, $log) = mapErrorCode($code);
    $data = array(
        &apos;level&apos; =&gt; $log,
        &apos;code&apos; =&gt; $code,
        &apos;error&apos; =&gt; $error,
        &apos;description&apos; =&gt; $description,
        &apos;file&apos; =&gt; $file,
        &apos;line&apos; =&gt; $line,
        &apos;context&apos; =&gt; $context,
        &apos;path&apos; =&gt; $file,
        &apos;message&apos; =&gt; $error . &apos; (&apos; . $code . &apos;): &apos; . $description . &apos; in [&apos; . $file . &apos;, line &apos; . $line . &apos;]&apos;
    );
    return fileLog($data);
}

/**
 * This method is used to write data in file
 * @param mixed $logData
 * @param string $fileName
 * @return boolean
 */
function fileLog($logData, $fileName = ERROR_LOG_FILE) {
    $fh = fopen($fileName, &apos;a+&apos;);
    if (is_array($logData)) {
        $logData = print_r($logData, 1);
    }
    $status = fwrite($fh, $logData);
    fclose($fh);
    return ($status) ? true : false;
}

/**
 * Map an error code into an Error word, and log location.
 *
 * @param int $code Error code to map
 * @return array Array of error word, and log location.
 */
function mapErrorCode($code) {
    $error = $log = null;
    switch ($code) {
        case E_PARSE:
        case E_ERROR:
        case E_CORE_ERROR:
        case E_COMPILE_ERROR:
        case E_USER_ERROR:
            $error = &apos;Fatal Error&apos;;
            $log = LOG_ERR;
            break;
        case E_WARNING:
        case E_USER_WARNING:
        case E_COMPILE_WARNING:
        case E_RECOVERABLE_ERROR:
            $error = &apos;Warning&apos;;
            $log = LOG_WARNING;
            break;
        case E_NOTICE:
        case E_USER_NOTICE:
            $error = &apos;Notice&apos;;
            $log = LOG_NOTICE;
            break;
        case E_STRICT:
            $error = &apos;Strict&apos;;
            $log = LOG_NOTICE;
            break;
        case E_DEPRECATED:
        case E_USER_DEPRECATED:
            $error = &apos;Deprecated&apos;;
            $log = LOG_NOTICE;
            break;
        default :
            break;
    }
    return array($error, $log);
}

//calling custom error handler
set_error_handler("handleError");

print_r($arra); //undefined variable
print_r($dssdfdfgg); //undefined variable
include_once &apos;file.php&apos;; //No such file or directory
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.set-error-handler.php)

**[To root](/README.md)**