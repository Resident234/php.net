# $_SERVER




<div class="phpcode"><span class="html">
Just a PHP file to put on your local server (as I don&apos;t have enough memory)
<br>
<br> <span class="default">&lt;?php
<br> $indicesServer </span><span class="keyword">= array(</span><span class="string">&apos;PHP_SELF&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;argv&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;argc&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;GATEWAY_INTERFACE&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_ADDR&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_NAME&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_SOFTWARE&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_PROTOCOL&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REQUEST_METHOD&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REQUEST_TIME&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REQUEST_TIME_FLOAT&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;QUERY_STRING&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;DOCUMENT_ROOT&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_ACCEPT&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_ACCEPT_CHARSET&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_ACCEPT_ENCODING&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_ACCEPT_LANGUAGE&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_CONNECTION&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_HOST&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_REFERER&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;HTTPS&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REMOTE_ADDR&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REMOTE_HOST&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REMOTE_PORT&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REMOTE_USER&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REDIRECT_REMOTE_USER&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SCRIPT_FILENAME&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_ADMIN&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_PORT&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SERVER_SIGNATURE&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;PATH_TRANSLATED&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;SCRIPT_NAME&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;REQUEST_URI&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;PHP_AUTH_DIGEST&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;PHP_AUTH_USER&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;PHP_AUTH_PW&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;AUTH_TYPE&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;PATH_INFO&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;ORIG_PATH_INFO&apos;</span><span class="keyword">) ;
<br>
<br>echo </span><span class="string">&apos;&lt;table cellpadding=&quot;10&quot;&gt;&apos; </span><span class="keyword">;
<br>foreach (</span><span class="default">$indicesServer </span><span class="keyword">as </span><span class="default">$arg</span><span class="keyword">) {
<br>&#xA0; &#xA0; if (isset(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="default">$arg</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;tr&gt;&lt;td&gt;&apos;</span><span class="keyword">.</span><span class="default">$arg</span><span class="keyword">.</span><span class="string">&apos;&lt;/td&gt;&lt;td&gt;&apos; </span><span class="keyword">. </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="default">$arg</span><span class="keyword">] . </span><span class="string">&apos;&lt;/td&gt;&lt;/tr&gt;&apos; </span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;tr&gt;&lt;td&gt;&apos;</span><span class="keyword">.</span><span class="default">$arg</span><span class="keyword">.</span><span class="string">&apos;&lt;/td&gt;&lt;td&gt;-&lt;/td&gt;&lt;/tr&gt;&apos; </span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>echo </span><span class="string">&apos;&lt;/table&gt;&apos; </span><span class="keyword">;
<br>
<br></span><span class="comment">/*
<br>
<br>That will give you the result of each variable like (if the file is server_indices.php at the root and Apache Web directory is in E:\web) : 
<br>
<br>PHP_SELF&#xA0; &#xA0; /server_indices.php
<br>argv&#xA0; &#xA0; -
<br>argc&#xA0; &#xA0; -
<br>GATEWAY_INTERFACE&#xA0; &#xA0; CGI/1.1
<br>SERVER_ADDR&#xA0; &#xA0; 127.0.0.1
<br>SERVER_NAME&#xA0; &#xA0; localhost
<br>SERVER_SOFTWARE&#xA0; &#xA0; Apache/2.2.22 (Win64) PHP/5.3.13
<br>SERVER_PROTOCOL&#xA0; &#xA0; HTTP/1.1
<br>REQUEST_METHOD&#xA0; &#xA0; GET
<br>REQUEST_TIME&#xA0; &#xA0; 1361542579
<br>REQUEST_TIME_FLOAT&#xA0; &#xA0; -
<br>QUERY_STRING&#xA0; &#xA0; 
<br>DOCUMENT_ROOT&#xA0; &#xA0; E:/web/
<br>HTTP_ACCEPT&#xA0; &#xA0; text/html,application/xhtml+xml,application/xml;q=0.9,*/</span><span class="keyword">*;</span><span class="default">q</span><span class="keyword">=</span><span class="default">0.8
<br>HTTP_ACCEPT_CHARSET&#xA0; &#xA0; ISO</span><span class="keyword">-</span><span class="default">8859</span><span class="keyword">-</span><span class="default">1</span><span class="keyword">,</span><span class="default">utf</span><span class="keyword">-</span><span class="default">8</span><span class="keyword">;</span><span class="default">q</span><span class="keyword">=</span><span class="default">0.7</span><span class="keyword">,*;</span><span class="default">q</span><span class="keyword">=</span><span class="default">0.3
<br>HTTP_ACCEPT_ENCODING&#xA0; &#xA0; gzip</span><span class="keyword">,</span><span class="default">deflate</span><span class="keyword">,</span><span class="default">sdch
<br>HTTP_ACCEPT_LANGUAGE&#xA0; &#xA0; fr</span><span class="keyword">-</span><span class="default">FR</span><span class="keyword">,</span><span class="default">fr</span><span class="keyword">;</span><span class="default">q</span><span class="keyword">=</span><span class="default">0.8</span><span class="keyword">,</span><span class="default">en</span><span class="keyword">-</span><span class="default">US</span><span class="keyword">;</span><span class="default">q</span><span class="keyword">=</span><span class="default">0.6</span><span class="keyword">,</span><span class="default">en</span><span class="keyword">;</span><span class="default">q</span><span class="keyword">=</span><span class="default">0.4
<br>HTTP_CONNECTION&#xA0; &#xA0; keep</span><span class="keyword">-</span><span class="default">alive
<br>HTTP_HOST&#xA0; &#xA0; localhost
<br>HTTP_REFERER&#xA0; &#xA0; http</span><span class="keyword">:</span><span class="comment">//localhost/
<br></span><span class="default">HTTP_USER_AGENT&#xA0; &#xA0; Mozilla</span><span class="keyword">/</span><span class="default">5.0 </span><span class="keyword">(</span><span class="default">Windows NT 6.1</span><span class="keyword">; </span><span class="default">WOW64</span><span class="keyword">) </span><span class="default">AppleWebKit</span><span class="keyword">/</span><span class="default">537.17 </span><span class="keyword">(</span><span class="default">KHTML</span><span class="keyword">, </span><span class="default">like Gecko</span><span class="keyword">) </span><span class="default">Chrome</span><span class="keyword">/</span><span class="default">24.0.1312.57 Safari</span><span class="keyword">/</span><span class="default">537.17
<br>HTTPS&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">REMOTE_ADDR&#xA0; &#xA0; 127.0.0.1
<br>REMOTE_HOST&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">REMOTE_PORT&#xA0; &#xA0; 65037
<br>REMOTE_USER&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">REDIRECT_REMOTE_USER&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">SCRIPT_FILENAME&#xA0; &#xA0; E</span><span class="keyword">:/</span><span class="default">web</span><span class="keyword">/</span><span class="default">server_indices</span><span class="keyword">.</span><span class="default">php
<br>SERVER_ADMIN&#xA0; &#xA0; myemail</span><span class="keyword">@</span><span class="default">personal</span><span class="keyword">.</span><span class="default">us
<br>SERVER_PORT&#xA0; &#xA0; 80
<br>SERVER_SIGNATURE&#xA0; &#xA0; 
<br>PATH_TRANSLATED&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">SCRIPT_NAME&#xA0; &#xA0; </span><span class="keyword">/</span><span class="default">server_indices</span><span class="keyword">.</span><span class="default">php
<br>REQUEST_URI&#xA0; &#xA0; </span><span class="keyword">/</span><span class="default">server_indices</span><span class="keyword">.</span><span class="default">php
<br>PHP_AUTH_DIGEST&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">PHP_AUTH_USER&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">PHP_AUTH_PW&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">AUTH_TYPE&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">PATH_INFO&#xA0; &#xA0; </span><span class="keyword">-
<br></span><span class="default">ORIG_PATH_INFO&#xA0; &#xA0; </span><span class="keyword">-
<br>
<br>*/
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
1. All elements of the $_SERVER array whose keys begin with &apos;HTTP_&apos; come from HTTP request headers and are not to be trusted.<br><br>2. All HTTP headers sent to the script are made available through the $_SERVER array, with names prefixed by &apos;HTTP_&apos;.<br><br>3. $_SERVER[&apos;PHP_SELF&apos;] is dangerous if misused. If login.php/nearly_arbitrary_string is requested, $_SERVER[&apos;PHP_SELF&apos;] will contain not just login.php, but the entire login.php/nearly_arbitrary_string. If you&apos;ve printed $_SERVER[&apos;PHP_SELF&apos;] as the value of the action attribute of your form tag without performing HTML encoding, an attacker can perform XSS attacks by offering users a link to your site such as this:<br><br>&lt;a href=&apos;<a href="http://www.example.com/login.php/" rel="nofollow" target="_blank">http://www.example.com/login.php/</a>&quot;&gt;&lt;script type=&quot;text/javascript&quot;&gt;...&lt;/script&gt;&lt;span a=&quot;&apos;&gt;Example.com&lt;/a&gt;<br><br>The javascript block would define an event handler function and bind it to the form&apos;s submit event. This event handler would load via an &lt;img&gt; tag an external file, with the submitted username and password as parameters.<br><br>Use $_SERVER[&apos;SCRIPT_NAME&apos;] instead of $_SERVER[&apos;PHP_SELF&apos;]. HTML encode every string sent to the browser that should not be interpreted as HTML, unless you are absolutely certain that it cannot contain anything that the browser can interpret as HTML.</span>
</div>
  

#


<div class="phpcode"><span class="html">
As PHP $_SERVER var is populated with a lot of vars, I think it&apos;s important to say that it&apos;s also populated with environment vars.<br><br>For example, with a PHP script, we can have this:<br><br>&#xA0; &#xA0; MY_ENV_VAR=Hello php -r &apos;echo $_SERVER[&quot;MY_ENV_VAR&quot;];&apos;<br>&#xA0; &#xA0; <br>Will show &quot;Hello&quot;.<br><br>But, internally, PHP makes sure that &quot;internal&quot; keys in $_SERVER are not overriden, so you wouldn&apos;t be able to do something like this:<br><br>&#xA0; &#xA0; REQUEST_TIME=Hello php -r &apos;var_dump($_SERVER[&quot;REQUEST_TIME&quot;]);&apos;<br>&#xA0; &#xA0; <br>Will show something like 1492897785<br><br>However, a lot of vars are still vulnerable from environment injection.<br><br>I created a gist here ( <a href="https://gist.github.com/Pierstoval/f287d3e61252e791a943dd73874ab5ee" rel="nofollow" target="_blank">https://gist.github.com/Pierstoval/f287d3e61252e791a943dd73874ab5ee</a> ) with my PHP configuration on windows with PHP7.0.15 on WSL with bash, the results are that the only &quot;safe&quot; vars are the following:<br><br>PHP_SELF<br>SCRIPT_NAME<br>SCRIPT_FILENAME<br>PATH_TRANSLATED<br>DOCUMENT_ROOT<br>REQUEST_TIME_FLOAT<br>REQUEST_TIME<br>argv<br>argc<br><br>All the rest can be overriden with environment vars, which is not very cool actually because it can break PHP applications sometimes...<br><br>(and I only tested on CLI, I had no patience to test with Apache mod_php or Nginx + PHP-FPM, but I can imagine that not a lot of $_SERVER properties are &quot;that&quot; secure...)</span>
</div>
  

#


<div class="phpcode"><span class="html">
An even *more* improved version...<br><br><span class="default">&lt;?php<br>phpinfo</span><span class="keyword">(</span><span class="default">32</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If requests to your PHP script send a header &quot;Content-Type&quot; or/ &quot;Content-Length&quot; it will, contrary to regular HTTP headers, not appear in $_SERVER as $_SERVER[&apos;HTTP_CONTENT_TYPE&apos;]. PHP removes these (per CGI/1.1 specification[1]) from the HTTP_ match group.<br><br>They are still accessible, but only if the request was a POST request. When it is, it&apos;ll be available as:<br>$_SERVER[&apos;CONTENT_LENGTH&apos;]<br>$_SERVER[&apos;CONTENT_TYPE&apos;]<br><br>[1] <a href="https://www.ietf.org/rfc/rfc3875" rel="nofollow" target="_blank">https://www.ietf.org/rfc/rfc3875</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
You have missed &apos;REDIRECT_STATUS&apos;<br><br>Very useful if you point all your error pages to the same file.<br><br>File; .htaccess<br># .htaccess file.<br><br>ErrorDocument 404 /error-msg.php<br>ErrorDocument 500 /error-msg.php<br>ErrorDocument 400 /error-msg.php<br>ErrorDocument 401 /error-msg.php<br>ErrorDocument 403 /error-msg.php<br># End of file.<br><br>File; error-msg.php<br><span class="default">&lt;?php<br>&#xA0; $HttpStatus </span><span class="keyword">= </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&quot;REDIRECT_STATUS&quot;</span><span class="keyword">] ;<br>&#xA0; if(</span><span class="default">$HttpStatus</span><span class="keyword">==</span><span class="default">200</span><span class="keyword">) {print </span><span class="string">&quot;Document has been processed and sent to you.&quot;</span><span class="keyword">;}<br>&#xA0; if(</span><span class="default">$HttpStatus</span><span class="keyword">==</span><span class="default">400</span><span class="keyword">) {print </span><span class="string">&quot;Bad HTTP request &quot;</span><span class="keyword">;}<br>&#xA0; if(</span><span class="default">$HttpStatus</span><span class="keyword">==</span><span class="default">401</span><span class="keyword">) {print </span><span class="string">&quot;Unauthorized - Iinvalid password&quot;</span><span class="keyword">;}<br>&#xA0; if(</span><span class="default">$HttpStatus</span><span class="keyword">==</span><span class="default">403</span><span class="keyword">) {print </span><span class="string">&quot;Forbidden&quot;</span><span class="keyword">;}<br>&#xA0; if(</span><span class="default">$HttpStatus</span><span class="keyword">==</span><span class="default">500</span><span class="keyword">) {print </span><span class="string">&quot;Internal Server Error&quot;</span><span class="keyword">;}<br>&#xA0; if(</span><span class="default">$HttpStatus</span><span class="keyword">==</span><span class="default">418</span><span class="keyword">) {print </span><span class="string">&quot;I&apos;m a teapot! - This is a real value, defined in 1998&quot;</span><span class="keyword">;}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When using the $_SERVER[&apos;SERVER_NAME&apos;] variable in an apache virtual host setup with a ServerAlias directive, be sure to check the UseCanonicalName apache directive.&#xA0; If it is On, this variable will always have the apache ServerName value.&#xA0; If it is Off, it will have the value given by the headers sent by the browser.<br><br>Depending on what you want to do the content of this variable, put in On or Off.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are serving from behind a proxy server, you will almost certainly save time by looking at what these $_SERVER variables do on your machine behind the proxy.&#xA0;&#xA0; <br><br>$_SERVER[&apos;HTTP_X_FORWARDED_FOR&apos;] in place of $_SERVER[&apos;REMOTE_ADDR&apos;]<br><br>$_SERVER[&apos;HTTP_X_FORWARDED_HOST&apos;] and <br>$_SERVER[&apos;HTTP_X_FORWARDED_SERVER&apos;] in place of (at least in our case,) $_SERVER[&apos;SERVER_NAME&apos;]</span>
</div>
  

#


<div class="phpcode"><span class="html">
A table of everything in the $_SERVER array can be found near the bottom of the output of phpinfo();</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s worth noting that $_SERVER variables get created for any HTTP request headers, including those you might invent:<br><br>If the browser sends an HTTP request header of:<br>X-Debug-Custom: some string<br><br>Then:<br><br><span class="default">&lt;?php<br>$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_X_DEBUG_CUSTOM&apos;</span><span class="keyword">]; </span><span class="comment">// &quot;some string&quot;<br></span><span class="default">?&gt;<br></span><br>There are better ways to identify the HTTP request headers sent by the browser, but this is convenient if you know what to expect from, for example, an AJAX script with custom headers.<br><br>Works in PHP5 on Apache with mod_php.&#xA0; Don&apos;t know if this is true from other environments.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.server.php)

**[⬆ to root](/)**