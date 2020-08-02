# set_error_handler



By this function alone you can not catch fatal errors, there is a simple work around. Below is part of my error.php file which handles errors and exceptions in the application. Before someone complains I&apos;ll add that I do not care that I am using globals, this file is part of my mini framework and without the &apos;config&apos; variable the application would crash anyways.<br><br>

```
<?php

/**
 * Error handler, passes flow over the exception logger with new ErrorException.
 */
function log_error( $num, $str, $file, $line, $context = null )
{
    log_exception( new ErrorException( $str, 0, $num, $file, $line ) );
}

/**
 * Uncaught exception handler.
 */
function log_exception( Exception $e )
{
    global $config;
    
    if ( $config["debug"] == true )
    {
        print "<div style='text-align: center;'>";
        print "<h2 style='color: rgb(190, 50, 50);'>Exception Occured:</h2>";
        print "<table style='width: 800px; display: inline-block;'>";
        print "<tr style='background-color:rgb(230,230,230);'><th style='width: 80px;'>Type</th><td>" . get_class( $e ) . "</td></tr>";
        print "<tr style='background-color:rgb(240,240,240);'><th>Message</th><td>{$e->getMessage()}</td></tr>";
        print "<tr style='background-color:rgb(230,230,230);'><th>File</th><td>{$e->getFile()}</td></tr>";
        print "<tr style='background-color:rgb(240,240,240);'><th>Line</th><td>{$e->getLine()}</td></tr>";
        print "</table></div>";
    }
    else
    {
        $message = "Type: " . get_class( $e ) . "; Message: {$e->getMessage()}; File: {$e->getFile()}; Line: {$e->getLine()};";
        file_put_contents( $config["app_dir"] . "/tmp/logs/exceptions.log", $message . PHP_EOL, FILE_APPEND );
        header( "Location: {$config["error_page"]}" );
    }
    
    exit();
}

/**
 * Checks for a fatal error, work around for set_error_handler not working on fatal errors.
 */
function check_for_fatal()
{
    $error = error_get_last();
    if ( $error["type"] == E_ERROR )
        log_error( $error["type"], $error["message"], $error["file"], $error["line"] );
}

register_shutdown_function( "check_for_fatal" );
set_error_handler( "log_error" );
set_exception_handler( "log_exception" );
ini_set( "display_errors", "off" );
error_reporting( E_ALL );?>
```
  

---



```
<?php
/**
 * throw exceptions based on E_* error types
 */
set_error_handler(function ($err_severity, $err_msg, $err_file, $err_line, array $err_context)
{
    // error was suppressed with the @-operator
    if (0 === error_reporting()) { return false;}
    switch($err_severity)
    {
        case E_ERROR:               throw new ErrorException            ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_WARNING:             throw new WarningException          ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_PARSE:               throw new ParseException            ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_NOTICE:              throw new NoticeException           ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_CORE_ERROR:          throw new CoreErrorException        ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_CORE_WARNING:        throw new CoreWarningException      ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_COMPILE_ERROR:       throw new CompileErrorException     ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_COMPILE_WARNING:     throw new CoreWarningException      ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_USER_ERROR:          throw new UserErrorException        ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_USER_WARNING:        throw new UserWarningException      ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_USER_NOTICE:         throw new UserNoticeException       ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_STRICT:              throw new StrictException           ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_RECOVERABLE_ERROR:   throw new RecoverableErrorException ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_DEPRECATED:          throw new DeprecatedException       ($err_msg, 0, $err_severity, $err_file, $err_line);
        case E_USER_DEPRECATED:     throw new UserDeprecatedException   ($err_msg, 0, $err_severity, $err_file, $err_line);
    }
});

class WarningException              extends ErrorException {}
class ParseException                extends ErrorException {}
class NoticeException               extends ErrorException {}
class CoreErrorException            extends ErrorException {}
class CoreWarningException          extends ErrorException {}
class CompileErrorException         extends ErrorException {}
class CompileWarningException       extends ErrorException {}
class UserErrorException            extends ErrorException {}
class UserWarningException          extends ErrorException {}
class UserNoticeException           extends ErrorException {}
class StrictException               extends ErrorException {}
class RecoverableErrorException     extends ErrorException {}
class DeprecatedException           extends ErrorException {}
class UserDeprecatedException       extends ErrorException {}?>
```
  

---



```
<?php
/**
 * Used for logging all php notices,warings and etc in a file when error reporting
 * is set and display_errors is off
 * @uses used in prod env for logging all type of error of php code in a file for further debugging
 * and code performance
 * @author Aditya Mehrotra<aditycse@gmail.com>
 */
error_reporting(E_ALL);
ini_set("display_errors", "off");
define('ERROR_LOG_FILE', '/var/www/error.log');

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
        'level' => $log,
        'code' => $code,
        'error' => $error,
        'description' => $description,
        'file' => $file,
        'line' => $line,
        'context' => $context,
        'path' => $file,
        'message' => $error . ' (' . $code . '): ' . $description . ' in [' . $file . ', line ' . $line . ']'
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
    $fh = fopen($fileName, 'a+');
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
            $error = 'Fatal Error';
            $log = LOG_ERR;
            break;
        case E_WARNING:
        case E_USER_WARNING:
        case E_COMPILE_WARNING:
        case E_RECOVERABLE_ERROR:
            $error = 'Warning';
            $log = LOG_WARNING;
            break;
        case E_NOTICE:
        case E_USER_NOTICE:
            $error = 'Notice';
            $log = LOG_NOTICE;
            break;
        case E_STRICT:
            $error = 'Strict';
            $log = LOG_NOTICE;
            break;
        case E_DEPRECATED:
        case E_USER_DEPRECATED:
            $error = 'Deprecated';
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
include_once 'file.php'; //No such file or directory
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.set-error-handler.php)

**[To root](/README.md)**