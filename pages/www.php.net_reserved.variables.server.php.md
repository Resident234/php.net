# $_SERVER



Just a PHP file to put on your local server (as I don&apos;t have enough memory)<br><br> 

```
<?php
 $indicesServer = array('PHP_SELF',
'argv',
'argc',
'GATEWAY_INTERFACE',
'SERVER_ADDR',
'SERVER_NAME',
'SERVER_SOFTWARE',
'SERVER_PROTOCOL',
'REQUEST_METHOD',
'REQUEST_TIME',
'REQUEST_TIME_FLOAT',
'QUERY_STRING',
'DOCUMENT_ROOT',
'HTTP_ACCEPT',
'HTTP_ACCEPT_CHARSET',
'HTTP_ACCEPT_ENCODING',
'HTTP_ACCEPT_LANGUAGE',
'HTTP_CONNECTION',
'HTTP_HOST',
'HTTP_REFERER',
'HTTP_USER_AGENT',
'HTTPS',
'REMOTE_ADDR',
'REMOTE_HOST',
'REMOTE_PORT',
'REMOTE_USER',
'REDIRECT_REMOTE_USER',
'SCRIPT_FILENAME',
'SERVER_ADMIN',
'SERVER_PORT',
'SERVER_SIGNATURE',
'PATH_TRANSLATED',
'SCRIPT_NAME',
'REQUEST_URI',
'PHP_AUTH_DIGEST',
'PHP_AUTH_USER',
'PHP_AUTH_PW',
'AUTH_TYPE',
'PATH_INFO',
'ORIG_PATH_INFO') ;

echo '&lt;table cellpadding="10"&gt;' ;
foreach ($indicesServer as $arg) {
    if (isset($_SERVER[$arg])) {
        echo '&lt;tr&gt;&lt;td&gt;'.$arg.'&lt;/td&gt;&lt;td&gt;' . $_SERVER[$arg] . '&lt;/td&gt;&lt;/tr&gt;' ;
    }
    else {
        echo '&lt;tr&gt;&lt;td&gt;'.$arg.'&lt;/td&gt;&lt;td&gt;-&lt;/td&gt;&lt;/tr&gt;' ;
    }
}
echo '&lt;/table&gt;' ;

/*

That will give you the result of each variable like (if the file is server_indices.php at the root and Apache Web directory is in E:\web) : 

PHP_SELF    /server_indices.php
argv    -
argc    -
GATEWAY_INTERFACE    CGI/1.1
SERVER_ADDR    127.0.0.1
SERVER_NAME    localhost
SERVER_SOFTWARE    Apache/2.2.22 (Win64) PHP/5.3.13
SERVER_PROTOCOL    HTTP/1.1
REQUEST_METHOD    GET
REQUEST_TIME    1361542579
REQUEST_TIME_FLOAT    -
QUERY_STRING    
DOCUMENT_ROOT    E:/web/
HTTP_ACCEPT    text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
HTTP_ACCEPT_CHARSET    ISO-8859-1,utf-8;q=0.7,*;q=0.3
HTTP_ACCEPT_ENCODING    gzip,deflate,sdch
HTTP_ACCEPT_LANGUAGE    fr-FR,fr;q=0.8,en-US;q=0.6,en;q=0.4
HTTP_CONNECTION    keep-alive
HTTP_HOST    localhost
HTTP_REFERER    http://localhost/
HTTP_USER_AGENT    Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.17 (KHTML, like Gecko) Chrome/24.0.1312.57 Safari/537.17
HTTPS    -
REMOTE_ADDR    127.0.0.1
REMOTE_HOST    -
REMOTE_PORT    65037
REMOTE_USER    -
REDIRECT_REMOTE_USER    -
SCRIPT_FILENAME    E:/web/server_indices.php
SERVER_ADMIN    myemail@personal.us
SERVER_PORT    80
SERVER_SIGNATURE    
PATH_TRANSLATED    -
SCRIPT_NAME    /server_indices.php
REQUEST_URI    /server_indices.php
PHP_AUTH_DIGEST    -
PHP_AUTH_USER    -
PHP_AUTH_PW    -
AUTH_TYPE    -
PATH_INFO    -
ORIG_PATH_INFO    -

*/
?>
```
  

#

1. All elements of the $_SERVER array whose keys begin with &apos;HTTP_&apos; come from HTTP request headers and are not to be trusted.<br><br>2. All HTTP headers sent to the script are made available through the $_SERVER array, with names prefixed by &apos;HTTP_&apos;.<br><br>3. $_SERVER[&apos;PHP_SELF&apos;] is dangerous if misused. If login.php/nearly_arbitrary_string is requested, $_SERVER[&apos;PHP_SELF&apos;] will contain not just login.php, but the entire login.php/nearly_arbitrary_string. If you&apos;ve printed $_SERVER[&apos;PHP_SELF&apos;] as the value of the action attribute of your form tag without performing HTML encoding, an attacker can perform XSS attacks by offering users a link to your site such as this:<br><br>&lt;a href=&apos;http://www.example.com/login.php/"&gt;&lt;script type="text/javascript"&gt;...&lt;/script&gt;&lt;span a="&apos;&gt;Example.com&lt;/a&gt;<br><br>The javascript block would define an event handler function and bind it to the form&apos;s submit event. This event handler would load via an &lt;img&gt; tag an external file, with the submitted username and password as parameters.<br><br>Use $_SERVER[&apos;SCRIPT_NAME&apos;] instead of $_SERVER[&apos;PHP_SELF&apos;]. HTML encode every string sent to the browser that should not be interpreted as HTML, unless you are absolutely certain that it cannot contain anything that the browser can interpret as HTML.  

#

As PHP $_SERVER var is populated with a lot of vars, I think it&apos;s important to say that it&apos;s also populated with environment vars.<br><br>For example, with a PHP script, we can have this:<br><br>    MY_ENV_VAR=Hello php -r &apos;echo $_SERVER["MY_ENV_VAR"];&apos;<br>    <br>Will show "Hello".<br><br>But, internally, PHP makes sure that "internal" keys in $_SERVER are not overriden, so you wouldn&apos;t be able to do something like this:<br><br>    REQUEST_TIME=Hello php -r &apos;var_dump($_SERVER["REQUEST_TIME"]);&apos;<br>    <br>Will show something like 1492897785<br><br>However, a lot of vars are still vulnerable from environment injection.<br><br>I created a gist here ( https://gist.github.com/Pierstoval/f287d3e61252e791a943dd73874ab5ee ) with my PHP configuration on windows with PHP7.0.15 on WSL with bash, the results are that the only "safe" vars are the following:<br><br>PHP_SELF<br>SCRIPT_NAME<br>SCRIPT_FILENAME<br>PATH_TRANSLATED<br>DOCUMENT_ROOT<br>REQUEST_TIME_FLOAT<br>REQUEST_TIME<br>argv<br>argc<br><br>All the rest can be overriden with environment vars, which is not very cool actually because it can break PHP applications sometimes...<br><br>(and I only tested on CLI, I had no patience to test with Apache mod_php or Nginx + PHP-FPM, but I can imagine that not a lot of $_SERVER properties are "that" secure...)  

#

An even *more* improved version...<br><br>

```
<?php
phpinfo(32);
?>
```
  

#

If requests to your PHP script send a header "Content-Type" or/ "Content-Length" it will, contrary to regular HTTP headers, not appear in $_SERVER as $_SERVER[&apos;HTTP_CONTENT_TYPE&apos;]. PHP removes these (per CGI/1.1 specification[1]) from the HTTP_ match group.<br><br>They are still accessible, but only if the request was a POST request. When it is, it&apos;ll be available as:<br>$_SERVER[&apos;CONTENT_LENGTH&apos;]<br>$_SERVER[&apos;CONTENT_TYPE&apos;]<br><br>[1] https://www.ietf.org/rfc/rfc3875  

#

You have missed &apos;REDIRECT_STATUS&apos;<br><br>Very useful if you point all your error pages to the same file.<br><br>File; .htaccess<br># .htaccess file.<br><br>ErrorDocument 404 /error-msg.php<br>ErrorDocument 500 /error-msg.php<br>ErrorDocument 400 /error-msg.php<br>ErrorDocument 401 /error-msg.php<br>ErrorDocument 403 /error-msg.php<br># End of file.<br><br>File; error-msg.php<br>

```
<?php
  $HttpStatus = $_SERVER["REDIRECT_STATUS"] ;
  if($HttpStatus==200) {print "Document has been processed and sent to you.";}
  if($HttpStatus==400) {print "Bad HTTP request ";}
  if($HttpStatus==401) {print "Unauthorized - Iinvalid password";}
  if($HttpStatus==403) {print "Forbidden";}
  if($HttpStatus==500) {print "Internal Server Error";}
  if($HttpStatus==418) {print "I'm a teapot! - This is a real value, defined in 1998";}

?>
```
  

#

When using the $_SERVER[&apos;SERVER_NAME&apos;] variable in an apache virtual host setup with a ServerAlias directive, be sure to check the UseCanonicalName apache directive.  If it is On, this variable will always have the apache ServerName value.  If it is Off, it will have the value given by the headers sent by the browser.<br><br>Depending on what you want to do the content of this variable, put in On or Off.  

#

If you are serving from behind a proxy server, you will almost certainly save time by looking at what these $_SERVER variables do on your machine behind the proxy.   <br><br>$_SERVER[&apos;HTTP_X_FORWARDED_FOR&apos;] in place of $_SERVER[&apos;REMOTE_ADDR&apos;]<br><br>$_SERVER[&apos;HTTP_X_FORWARDED_HOST&apos;] and <br>$_SERVER[&apos;HTTP_X_FORWARDED_SERVER&apos;] in place of (at least in our case,) $_SERVER[&apos;SERVER_NAME&apos;]  

#

A table of everything in the $_SERVER array can be found near the bottom of the output of phpinfo();  

#

It&apos;s worth noting that $_SERVER variables get created for any HTTP request headers, including those you might invent:<br><br>If the browser sends an HTTP request header of:<br>X-Debug-Custom: some string<br><br>Then:<br><br>

```
<?php
$_SERVER['HTTP_X_DEBUG_CUSTOM']; // "some string"
?>
```
<br><br>There are better ways to identify the HTTP request headers sent by the browser, but this is convenient if you know what to expect from, for example, an AJAX script with custom headers.<br><br>Works in PHP5 on Apache with mod_php.  Don&apos;t know if this is true from other environments.  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.server.php)

**[To root](/README.md)**