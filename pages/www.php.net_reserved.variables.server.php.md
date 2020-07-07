# $_SERVER





Just a PHP file to put on your local server (as I don&apos;t have enough memory)



 

```
<?php

 $indicesServer = array(&apos;PHP_SELF&apos;,

&apos;argv&apos;,

&apos;argc&apos;,

&apos;GATEWAY_INTERFACE&apos;,

&apos;SERVER_ADDR&apos;,

&apos;SERVER_NAME&apos;,

&apos;SERVER_SOFTWARE&apos;,

&apos;SERVER_PROTOCOL&apos;,

&apos;REQUEST_METHOD&apos;,

&apos;REQUEST_TIME&apos;,

&apos;REQUEST_TIME_FLOAT&apos;,

&apos;QUERY_STRING&apos;,

&apos;DOCUMENT_ROOT&apos;,

&apos;HTTP_ACCEPT&apos;,

&apos;HTTP_ACCEPT_CHARSET&apos;,

&apos;HTTP_ACCEPT_ENCODING&apos;,

&apos;HTTP_ACCEPT_LANGUAGE&apos;,

&apos;HTTP_CONNECTION&apos;,

&apos;HTTP_HOST&apos;,

&apos;HTTP_REFERER&apos;,

&apos;HTTP_USER_AGENT&apos;,

&apos;HTTPS&apos;,

&apos;REMOTE_ADDR&apos;,

&apos;REMOTE_HOST&apos;,

&apos;REMOTE_PORT&apos;,

&apos;REMOTE_USER&apos;,

&apos;REDIRECT_REMOTE_USER&apos;,

&apos;SCRIPT_FILENAME&apos;,

&apos;SERVER_ADMIN&apos;,

&apos;SERVER_PORT&apos;,

&apos;SERVER_SIGNATURE&apos;,

&apos;PATH_TRANSLATED&apos;,

&apos;SCRIPT_NAME&apos;,

&apos;REQUEST_URI&apos;,

&apos;PHP_AUTH_DIGEST&apos;,

&apos;PHP_AUTH_USER&apos;,

&apos;PHP_AUTH_PW&apos;,

&apos;AUTH_TYPE&apos;,

&apos;PATH_INFO&apos;,

&apos;ORIG_PATH_INFO&apos;) ;



echo &apos;&lt;table cellpadding=&quot;10&quot;&gt;&apos; ;

foreach ($indicesServer as $arg) {

&#xA0; &#xA0; if (isset($_SERVER[$arg])) {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&lt;tr&gt;&lt;td&gt;&apos;.$arg.&apos;&lt;/td&gt;&lt;td&gt;&apos; . $_SERVER[$arg] . &apos;&lt;/td&gt;&lt;/tr&gt;&apos; ;

&#xA0; &#xA0; }

&#xA0; &#xA0; else {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&lt;tr&gt;&lt;td&gt;&apos;.$arg.&apos;&lt;/td&gt;&lt;td&gt;-&lt;/td&gt;&lt;/tr&gt;&apos; ;

&#xA0; &#xA0; }

}

echo &apos;&lt;/table&gt;&apos; ;



/*



That will give you the result of each variable like (if the file is server_indices.php at the root and Apache Web directory is in E:\web) : 



PHP_SELF&#xA0; &#xA0; /server_indices.php

argv&#xA0; &#xA0; -

argc&#xA0; &#xA0; -

GATEWAY_INTERFACE&#xA0; &#xA0; CGI/1.1

SERVER_ADDR&#xA0; &#xA0; 127.0.0.1

SERVER_NAME&#xA0; &#xA0; localhost

SERVER_SOFTWARE&#xA0; &#xA0; Apache/2.2.22 (Win64) PHP/5.3.13

SERVER_PROTOCOL&#xA0; &#xA0; HTTP/1.1

REQUEST_METHOD&#xA0; &#xA0; GET

REQUEST_TIME&#xA0; &#xA0; 1361542579

REQUEST_TIME_FLOAT&#xA0; &#xA0; -

QUERY_STRING&#xA0; &#xA0; 

DOCUMENT_ROOT&#xA0; &#xA0; E:/web/

HTTP_ACCEPT&#xA0; &#xA0; text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8

HTTP_ACCEPT_CHARSET&#xA0; &#xA0; ISO-8859-1,utf-8;q=0.7,*;q=0.3

HTTP_ACCEPT_ENCODING&#xA0; &#xA0; gzip,deflate,sdch

HTTP_ACCEPT_LANGUAGE&#xA0; &#xA0; fr-FR,fr;q=0.8,en-US;q=0.6,en;q=0.4

HTTP_CONNECTION&#xA0; &#xA0; keep-alive

HTTP_HOST&#xA0; &#xA0; localhost

HTTP_REFERER&#xA0; &#xA0; http://localhost/

HTTP_USER_AGENT&#xA0; &#xA0; Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.17 (KHTML, like Gecko) Chrome/24.0.1312.57 Safari/537.17

HTTPS&#xA0; &#xA0; -

REMOTE_ADDR&#xA0; &#xA0; 127.0.0.1

REMOTE_HOST&#xA0; &#xA0; -

REMOTE_PORT&#xA0; &#xA0; 65037

REMOTE_USER&#xA0; &#xA0; -

REDIRECT_REMOTE_USER&#xA0; &#xA0; -

SCRIPT_FILENAME&#xA0; &#xA0; E:/web/server_indices.php

SERVER_ADMIN&#xA0; &#xA0; myemail@personal.us

SERVER_PORT&#xA0; &#xA0; 80

SERVER_SIGNATURE&#xA0; &#xA0; 

PATH_TRANSLATED&#xA0; &#xA0; -

SCRIPT_NAME&#xA0; &#xA0; /server_indices.php

REQUEST_URI&#xA0; &#xA0; /server_indices.php

PHP_AUTH_DIGEST&#xA0; &#xA0; -

PHP_AUTH_USER&#xA0; &#xA0; -

PHP_AUTH_PW&#xA0; &#xA0; -

AUTH_TYPE&#xA0; &#xA0; -

PATH_INFO&#xA0; &#xA0; -

ORIG_PATH_INFO&#xA0; &#xA0; -



*/

?>
```



  

#



1. All elements of the $_SERVER array whose keys begin with &apos;HTTP_&apos; come from HTTP request headers and are not to be trusted.

2. All HTTP headers sent to the script are made available through the $_SERVER array, with names prefixed by &apos;HTTP_&apos;.

3. $_SERVER[&apos;PHP_SELF&apos;] is dangerous if misused. If login.php/nearly_arbitrary_string is requested, $_SERVER[&apos;PHP_SELF&apos;] will contain not just login.php, but the entire login.php/nearly_arbitrary_string. If you&apos;ve printed $_SERVER[&apos;PHP_SELF&apos;] as the value of the action attribute of your form tag without performing HTML encoding, an attacker can perform XSS attacks by offering users a link to your site such as this:

&lt;a href=&apos;http://www.example.com/login.php/&quot;&gt;&lt;script type=&quot;text/javascript&quot;&gt;...&lt;/script&gt;&lt;span a=&quot;&apos;&gt;Example.com&lt;/a&gt;

The javascript block would define an event handler function and bind it to the form&apos;s submit event. This event handler would load via an &lt;img&gt; tag an external file, with the submitted username and password as parameters.

Use $_SERVER[&apos;SCRIPT_NAME&apos;] instead of $_SERVER[&apos;PHP_SELF&apos;]. HTML encode every string sent to the browser that should not be interpreted as HTML, unless you are absolutely certain that it cannot contain anything that the browser can interpret as HTML.

  

#



As PHP $_SERVER var is populated with a lot of vars, I think it&apos;s important to say that it&apos;s also populated with environment vars.

For example, with a PHP script, we can have this:

&#xA0; &#xA0; MY_ENV_VAR=Hello php -r &apos;echo $_SERVER[&quot;MY_ENV_VAR&quot;];&apos;
&#xA0; &#xA0; 
Will show &quot;Hello&quot;.

But, internally, PHP makes sure that &quot;internal&quot; keys in $_SERVER are not overriden, so you wouldn&apos;t be able to do something like this:

&#xA0; &#xA0; REQUEST_TIME=Hello php -r &apos;var_dump($_SERVER[&quot;REQUEST_TIME&quot;]);&apos;
&#xA0; &#xA0; 
Will show something like 1492897785

However, a lot of vars are still vulnerable from environment injection.

I created a gist here ( https://gist.github.com/Pierstoval/f287d3e61252e791a943dd73874ab5ee ) with my PHP configuration on windows with PHP7.0.15 on WSL with bash, the results are that the only &quot;safe&quot; vars are the following:

PHP_SELF
SCRIPT_NAME
SCRIPT_FILENAME
PATH_TRANSLATED
DOCUMENT_ROOT
REQUEST_TIME_FLOAT
REQUEST_TIME
argv
argc

All the rest can be overriden with environment vars, which is not very cool actually because it can break PHP applications sometimes...

(and I only tested on CLI, I had no patience to test with Apache mod_php or Nginx + PHP-FPM, but I can imagine that not a lot of $_SERVER properties are &quot;that&quot; secure...)

  

#



An even *more* improved version...



```
<?php
phpinfo(32);
?>
```



  

#



If requests to your PHP script send a header &quot;Content-Type&quot; or/ &quot;Content-Length&quot; it will, contrary to regular HTTP headers, not appear in $_SERVER as $_SERVER[&apos;HTTP_CONTENT_TYPE&apos;]. PHP removes these (per CGI/1.1 specification[1]) from the HTTP_ match group.

They are still accessible, but only if the request was a POST request. When it is, it&apos;ll be available as:
$_SERVER[&apos;CONTENT_LENGTH&apos;]
$_SERVER[&apos;CONTENT_TYPE&apos;]

[1] https://www.ietf.org/rfc/rfc3875

  

#



You have missed &apos;REDIRECT_STATUS&apos;

Very useful if you point all your error pages to the same file.

File; .htaccess
# .htaccess file.

ErrorDocument 404 /error-msg.php
ErrorDocument 500 /error-msg.php
ErrorDocument 400 /error-msg.php
ErrorDocument 401 /error-msg.php
ErrorDocument 403 /error-msg.php
# End of file.

File; error-msg.php


```
<?php
&#xA0; $HttpStatus = $_SERVER[&quot;REDIRECT_STATUS&quot;] ;
&#xA0; if($HttpStatus==200) {print &quot;Document has been processed and sent to you.&quot;;}
&#xA0; if($HttpStatus==400) {print &quot;Bad HTTP request &quot;;}
&#xA0; if($HttpStatus==401) {print &quot;Unauthorized - Iinvalid password&quot;;}
&#xA0; if($HttpStatus==403) {print &quot;Forbidden&quot;;}
&#xA0; if($HttpStatus==500) {print &quot;Internal Server Error&quot;;}
&#xA0; if($HttpStatus==418) {print &quot;I&apos;m a teapot! - This is a real value, defined in 1998&quot;;}

?>
```



  

#



When using the $_SERVER[&apos;SERVER_NAME&apos;] variable in an apache virtual host setup with a ServerAlias directive, be sure to check the UseCanonicalName apache directive.&#xA0; If it is On, this variable will always have the apache ServerName value.&#xA0; If it is Off, it will have the value given by the headers sent by the browser.

Depending on what you want to do the content of this variable, put in On or Off.

  

#



If you are serving from behind a proxy server, you will almost certainly save time by looking at what these $_SERVER variables do on your machine behind the proxy.&#xA0;&#xA0; 

$_SERVER[&apos;HTTP_X_FORWARDED_FOR&apos;] in place of $_SERVER[&apos;REMOTE_ADDR&apos;]

$_SERVER[&apos;HTTP_X_FORWARDED_HOST&apos;] and 
$_SERVER[&apos;HTTP_X_FORWARDED_SERVER&apos;] in place of (at least in our case,) $_SERVER[&apos;SERVER_NAME&apos;]

  

#



A table of everything in the $_SERVER array can be found near the bottom of the output of phpinfo();

  

#



It&apos;s worth noting that $_SERVER variables get created for any HTTP request headers, including those you might invent:

If the browser sends an HTTP request header of:
X-Debug-Custom: some string

Then:



```
<?php
$_SERVER[&apos;HTTP_X_DEBUG_CUSTOM&apos;]; // &quot;some string&quot;
?>
```


There are better ways to identify the HTTP request headers sent by the browser, but this is convenient if you know what to expect from, for example, an AJAX script with custom headers.

Works in PHP5 on Apache with mod_php.&#xA0; Don&apos;t know if this is true from other environments.

  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.server.php)

**[To root](/README.md)**