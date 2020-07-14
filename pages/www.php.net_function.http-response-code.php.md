# http_response_code



If your version of PHP does not include this function:<br><br>

```
<?php

    if (!function_exists(&apos;http_response_code&apos;)) {
        function http_response_code($code = NULL) {

            if ($code !== NULL) {

                switch ($code) {
                    case 100: $text = &apos;Continue&apos;; break;
                    case 101: $text = &apos;Switching Protocols&apos;; break;
                    case 200: $text = &apos;OK&apos;; break;
                    case 201: $text = &apos;Created&apos;; break;
                    case 202: $text = &apos;Accepted&apos;; break;
                    case 203: $text = &apos;Non-Authoritative Information&apos;; break;
                    case 204: $text = &apos;No Content&apos;; break;
                    case 205: $text = &apos;Reset Content&apos;; break;
                    case 206: $text = &apos;Partial Content&apos;; break;
                    case 300: $text = &apos;Multiple Choices&apos;; break;
                    case 301: $text = &apos;Moved Permanently&apos;; break;
                    case 302: $text = &apos;Moved Temporarily&apos;; break;
                    case 303: $text = &apos;See Other&apos;; break;
                    case 304: $text = &apos;Not Modified&apos;; break;
                    case 305: $text = &apos;Use Proxy&apos;; break;
                    case 400: $text = &apos;Bad Request&apos;; break;
                    case 401: $text = &apos;Unauthorized&apos;; break;
                    case 402: $text = &apos;Payment Required&apos;; break;
                    case 403: $text = &apos;Forbidden&apos;; break;
                    case 404: $text = &apos;Not Found&apos;; break;
                    case 405: $text = &apos;Method Not Allowed&apos;; break;
                    case 406: $text = &apos;Not Acceptable&apos;; break;
                    case 407: $text = &apos;Proxy Authentication Required&apos;; break;
                    case 408: $text = &apos;Request Time-out&apos;; break;
                    case 409: $text = &apos;Conflict&apos;; break;
                    case 410: $text = &apos;Gone&apos;; break;
                    case 411: $text = &apos;Length Required&apos;; break;
                    case 412: $text = &apos;Precondition Failed&apos;; break;
                    case 413: $text = &apos;Request Entity Too Large&apos;; break;
                    case 414: $text = &apos;Request-URI Too Large&apos;; break;
                    case 415: $text = &apos;Unsupported Media Type&apos;; break;
                    case 500: $text = &apos;Internal Server Error&apos;; break;
                    case 501: $text = &apos;Not Implemented&apos;; break;
                    case 502: $text = &apos;Bad Gateway&apos;; break;
                    case 503: $text = &apos;Service Unavailable&apos;; break;
                    case 504: $text = &apos;Gateway Time-out&apos;; break;
                    case 505: $text = &apos;HTTP Version not supported&apos;; break;
                    default:
                        exit(&apos;Unknown http status code "&apos; . htmlentities($code) . &apos;"&apos;);
                    break;
                }

                $protocol = (isset($_SERVER[&apos;SERVER_PROTOCOL&apos;]) ? $_SERVER[&apos;SERVER_PROTOCOL&apos;] : &apos;HTTP/1.0&apos;);

                header($protocol . &apos; &apos; . $code . &apos; &apos; . $text);

                $GLOBALS[&apos;http_response_code&apos;] = $code;

            } else {

                $code = (isset($GLOBALS[&apos;http_response_code&apos;]) ? $GLOBALS[&apos;http_response_code&apos;] : 200);

            }

            return $code;

        }
    }

?>
```
<br><br>In this example I am using $GLOBALS, but you can use whatever storage mechanism you like... I don&apos;t think there is a way to return the current status code:<br><br>https://bugs.php.net/bug.php?id=52555<br><br>For reference the error codes I got from PHP&apos;s source code:<br><br>http://lxr.php.net/opengrok/xref/PHP_5_4/sapi/cgi/cgi_main.c#354<br><br>And how the current http header is sent, with the variables it uses:<br><br>http://lxr.php.net/opengrok/xref/PHP_5_4/main/SAPI.c#856  

#

Note that you can NOT set arbitrary response codes with this function, only those that are known to PHP (or the SAPI PHP is running on). <br><br>The following codes currently work as expected (with PHP running as Apache module):<br>200 &#x2013; 208, 226<br>300 &#x2013; 305, 307, 308<br>400 &#x2013; 417, 422 &#x2013; 424, 426, 428 &#x2013; 429, 431<br>500 &#x2013; 508, 510 &#x2013; 511<br><br>Codes 0, 100, 101, and 102 will be sent as "200 OK".<br><br>Everything else will result in "500 Internal Server Error".<br><br>If you want to send responses with a freestyle status line, you need to use the `header()` function:<br><br>

```
<?php header("HTTP/1.0 418 I&apos;m A Teapot"); ?>
```
  

#

You can also create a enum by extending the SplEnum class.<br>

```
<?php<br><br>/** HTTP status codes */<br>class HttpStatusCode extends SplEnum {<br>    const __default = self::OK;<br>    <br>    const SWITCHING_PROTOCOLS = 101;<br>    const OK = 200;<br>    const CREATED = 201;<br>    const ACCEPTED = 202;<br>    const NONAUTHORITATIVE_INFORMATION = 203;<br>    const NO_CONTENT = 204;<br>    const RESET_CONTENT = 205;<br>    const PARTIAL_CONTENT = 206;<br>    const MULTIPLE_CHOICES = 300;<br>    const MOVED_PERMANENTLY = 301;<br>    const MOVED_TEMPORARILY = 302;<br>    const SEE_OTHER = 303;<br>    const NOT_MODIFIED = 304;<br>    const USE_PROXY = 305;<br>    const BAD_REQUEST = 400;<br>    const UNAUTHORIZED = 401;<br>    const PAYMENT_REQUIRED = 402;<br>    const FORBIDDEN = 403;<br>    const NOT_FOUND = 404;<br>    const METHOD_NOT_ALLOWED = 405;<br>    const NOT_ACCEPTABLE = 406;<br>    const PROXY_AUTHENTICATION_REQUIRED = 407;<br>    const REQUEST_TIMEOUT = 408;<br>    const CONFLICT = 408;<br>    const GONE = 410;<br>    const LENGTH_REQUIRED = 411;<br>    const PRECONDITION_FAILED = 412;<br>    const REQUEST_ENTITY_TOO_LARGE = 413;<br>    const REQUESTURI_TOO_LARGE = 414;<br>    const UNSUPPORTED_MEDIA_TYPE = 415;<br>    const REQUESTED_RANGE_NOT_SATISFIABLE = 416;<br>    const EXPECTATION_FAILED = 417;<br>    const IM_A_TEAPOT = 418;<br>    const INTERNAL_SERVER_ERROR = 500;<br>    const NOT_IMPLEMENTED = 501;<br>    const BAD_GATEWAY = 502;<br>    const SERVICE_UNAVAILABLE = 503;<br>    const GATEWAY_TIMEOUT = 504;<br>    const HTTP_VERSION_NOT_SUPPORTED = 505;<br>}  

#

Status codes as an array:<br><br>

```
<?php
$http_status_codes = array(100 =&gt; "Continue", 101 =&gt; "Switching Protocols", 102 =&gt; "Processing", 200 =&gt; "OK", 201 =&gt; "Created", 202 =&gt; "Accepted", 203 =&gt; "Non-Authoritative Information", 204 =&gt; "No Content", 205 =&gt; "Reset Content", 206 =&gt; "Partial Content", 207 =&gt; "Multi-Status", 300 =&gt; "Multiple Choices", 301 =&gt; "Moved Permanently", 302 =&gt; "Found", 303 =&gt; "See Other", 304 =&gt; "Not Modified", 305 =&gt; "Use Proxy", 306 =&gt; "(Unused)", 307 =&gt; "Temporary Redirect", 308 =&gt; "Permanent Redirect", 400 =&gt; "Bad Request", 401 =&gt; "Unauthorized", 402 =&gt; "Payment Required", 403 =&gt; "Forbidden", 404 =&gt; "Not Found", 405 =&gt; "Method Not Allowed", 406 =&gt; "Not Acceptable", 407 =&gt; "Proxy Authentication Required", 408 =&gt; "Request Timeout", 409 =&gt; "Conflict", 410 =&gt; "Gone", 411 =&gt; "Length Required", 412 =&gt; "Precondition Failed", 413 =&gt; "Request Entity Too Large", 414 =&gt; "Request-URI Too Long", 415 =&gt; "Unsupported Media Type", 416 =&gt; "Requested Range Not Satisfiable", 417 =&gt; "Expectation Failed", 418 =&gt; "I&apos;m a teapot", 419 =&gt; "Authentication Timeout", 420 =&gt; "Enhance Your Calm", 422 =&gt; "Unprocessable Entity", 423 =&gt; "Locked", 424 =&gt; "Failed Dependency", 424 =&gt; "Method Failure", 425 =&gt; "Unordered Collection", 426 =&gt; "Upgrade Required", 428 =&gt; "Precondition Required", 429 =&gt; "Too Many Requests", 431 =&gt; "Request Header Fields Too Large", 444 =&gt; "No Response", 449 =&gt; "Retry With", 450 =&gt; "Blocked by Windows Parental Controls", 451 =&gt; "Unavailable For Legal Reasons", 494 =&gt; "Request Header Too Large", 495 =&gt; "Cert Error", 496 =&gt; "No Cert", 497 =&gt; "HTTP to HTTPS", 499 =&gt; "Client Closed Request", 500 =&gt; "Internal Server Error", 501 =&gt; "Not Implemented", 502 =&gt; "Bad Gateway", 503 =&gt; "Service Unavailable", 504 =&gt; "Gateway Timeout", 505 =&gt; "HTTP Version Not Supported", 506 =&gt; "Variant Also Negotiates", 507 =&gt; "Insufficient Storage", 508 =&gt; "Loop Detected", 509 =&gt; "Bandwidth Limit Exceeded", 510 =&gt; "Not Extended", 511 =&gt; "Network Authentication Required", 598 =&gt; "Network read timeout error", 599 =&gt; "Network connect timeout error");
?>
```
<br><br>Source: Wikipedia "List_of_HTTP_status_codes"  

#

The note above from "Anonymous" is wrong. I&apos;m running this behind the AWS Elastic Loadbalancer and trying the header(&apos;:&apos;.$error_code...) method mentioned above is treated as invalid HTTP.<br><br>The documentation for the header() function has the right way to implement this if you&apos;re still on &lt; php 5.4:<br><br>

```
<?php
header("HTTP/1.0 404 Not Found");
?>
```
  

#

If you don&apos;t have PHP 5.4 and want to change the returned status code, you can simply write:<br>

```
<?php
header(&apos;:&apos;, true, $statusCode);
?>
```
<br><br>The &apos;:&apos; are mandatory, or it won&apos;t work  

#

[Official documentation page](https://www.php.net/manual/en/function.http-response-code.php)

**[To root](/README.md)**