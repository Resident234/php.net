# curl_setopt




<div class="phpcode"><span class="html">
Please everyone, stop setting CURLOPT_SSL_VERIFYPEER to false or 0. If your PHP installation doesn&apos;t have an up-to-date CA root certificate bundle, download the one at the curl website and save it on your server:<br><br><a href="http://curl.haxx.se/docs/caextract.html" rel="nofollow" target="_blank">http://curl.haxx.se/docs/caextract.html</a><br><br>Then set a path to it in your php.ini file, e.g. on Windows:<br><br>curl.cainfo=c:\php\cacert.pem<br><br>Turning off CURLOPT_SSL_VERIFYPEER allows man in the middle (MITM) attacks, which you don&apos;t want!</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is important that anyone working with cURL and PHP keep in mind that not all of the CURLOPT and CURLINFO constants are documented. I always recommend reading the cURL documentation directly as it sometimes contains better information. The cURL API in tends to be fubar as well so do not expect things to be where you would normally logically look for them.<br><br>curl is especially difficult to work with when it comes to cookies. So I will talk about what I found with PHP 5.6 and curl 7.26.<br><br>If you want to manage cookies in memory without using files including reading, writing and clearing custom cookies then continue reading.<br><br>To start with, the way to enable in memory only cookies associated with a cURL handle you should use:<br><br>&#xA0; &#xA0; curl_setopt($curl, CURLOPT_COOKIEFILE, &quot;&quot;);<br><br>cURL likes to use magic strings in options as special commands. Rather than having an option to enable the cookie engine in memory it uses a magic string to do that. Although vaguely the documentation here mentions this however most people like me wouldn&apos;t even read that because a COOKIEFILE is the complete opposite of what we want.<br><br>To get the cookies for a curl handle you can use:<br><br>&#xA0; &#xA0; curl_getinfo($curl, CURLINFO_COOKIELIST);<br><br>This will give an array containing a string for each cookie. It is tab delimited and unfortunately you will have to parse it yourself if you want to do anything beyond copying the cookies.<br><br>To clear the in memory cookies for a cURL handle you can use:<br><br>&#xA0; &#xA0; curl_setopt($curl, CURLOPT_COOKIELIST, &quot;ALL&quot;);<br><br>This is a magic string. There are others in the cURL documentation. If a magic string isn&apos;t used, this field should take a cookie in the same string format as in getinfo for the cookielist constant. This can be used to delete individual cookies although it&apos;s not the most elegant API for doing so.<br><br>For copying cookies I recommend using curl_share_init.<br><br>You can also copy cookies from one handle to another like so:<br><br>&#xA0; &#xA0; foreach(curl_getinfo($curl_a, CURLINFO_COOKIELIST) as $cookie_line)<br>&#xA0; &#xA0; &#xA0; &#xA0; curl_setopt($curl, CURLOPT_COOKIELIST, $cookie_line);<br><br>An inelegant way to delete a cookie would be to skip the one you don&apos;t want. <br><br>I only recommend using COOKIELIST with magic strings because the cookie format is not secure or stable. You can inject tabs into at least path and name so it becomes impossible to parse reliably. If you must parse this then to keep it secure I recommend prohibiting more than 6 tabs in the content which probably isn&apos;t a big loss to most people.<br><br>A the absolute minimum for validation I would suggest:<br><br>&#xA0; &#xA0; /^([^\t]+\t){5}[^\t]+$/D<br><br>Here is the format:<br><br>&#xA0; &#xA0; #define SEP&#xA0; &quot;\t&quot;&#xA0; /* Tab separates the fields */<br> <br>&#xA0; &#xA0; char *my_cookie =<br>&#xA0; &#xA0; &#xA0; &quot;example.com&quot;&#xA0; &#xA0; /* Hostname */<br>&#xA0; &#xA0; &#xA0; SEP &quot;FALSE&quot;&#xA0; &#xA0; &#xA0; /* Include subdomains */<br>&#xA0; &#xA0; &#xA0; SEP &quot;/&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* Path */<br>&#xA0; &#xA0; &#xA0; SEP &quot;FALSE&quot;&#xA0; &#xA0; &#xA0; /* Secure */<br>&#xA0; &#xA0; &#xA0; SEP &quot;0&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* Expiry in epoch time format. 0 == Session */<br>&#xA0; &#xA0; &#xA0; SEP &quot;foo&quot;&#xA0; &#xA0; &#xA0; &#xA0; /* Name */<br>&#xA0; &#xA0; &#xA0; SEP &quot;bar&quot;;&#xA0; &#xA0; &#xA0;&#xA0; /* Value */</span>
</div>
  

#


<div class="phpcode"><span class="html">
Clarification on the callback methods:<br><br>- CURLOPT_HEADERFUNCTION is for handling header lines received *in the response*,<br>- CURLOPT_WRITEFUNCTION is for handling data received *from the response*,<br>- CURLOPT_READFUNCTION is for handling data passed along *in the request*.<br><br>The callback &quot;string&quot; can be any callable function, that includes the array(&amp;$obj, &apos;someMethodName&apos;) format.<br><br> -Philippe</span>
</div>
  

#


<div class="phpcode"><span class="html">
PUT requests are very simple, just make sure to specify a content-length header and set post fields as a string.
<br>
<br>Example:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">doPut</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="default">$fields</span><span class="keyword">)
<br>{
<br>&#xA0;&#xA0; </span><span class="default">$fields </span><span class="keyword">= (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$fields</span><span class="keyword">)) ? </span><span class="default">http_build_query</span><span class="keyword">(</span><span class="default">$fields</span><span class="keyword">) : </span><span class="default">$fields</span><span class="keyword">;
<br>
<br>&#xA0;&#xA0; if(</span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">))
<br>&#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_CUSTOMREQUEST</span><span class="keyword">, </span><span class="string">&apos;PUT&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">, array(</span><span class="string">&apos;Content-Length: &apos; </span><span class="keyword">. </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$fields</span><span class="keyword">)));
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_POSTFIELDS</span><span class="keyword">, </span><span class="default">$fields</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">= </span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLINFO_HTTP_CODE</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; return (int) </span><span class="default">$status</span><span class="keyword">;
<br>&#xA0;&#xA0; }
<br>&#xA0;&#xA0; else
<br>&#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0;&#xA0; }
<br>}
<br>
<br>if(</span><span class="default">doPut</span><span class="keyword">(</span><span class="string">&apos;<a href="http://example.com/api/a/b/c" rel="nofollow" target="_blank">http://example.com/api/a/b/c</a>&apos;</span><span class="keyword">, array(</span><span class="string">&apos;foo&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;bar&apos;</span><span class="keyword">)) == </span><span class="default">200</span><span class="keyword">)
<br>&#xA0;&#xA0; </span><span class="comment">// do something
<br></span><span class="keyword">else
<br>&#xA0;&#xA0; </span><span class="comment">// do something else.
<br></span><span class="default">?&gt;
<br></span>
<br>You can grab the request data on the other side with:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REQUEST_METHOD&apos;</span><span class="keyword">] == </span><span class="string">&apos;PUT&apos;</span><span class="keyword">)
<br>{
<br>&#xA0;&#xA0; </span><span class="default">parse_str</span><span class="keyword">(</span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;php://input&apos;</span><span class="keyword">), </span><span class="default">$requestData</span><span class="keyword">);
<br>
<br>&#xA0;&#xA0; </span><span class="comment">// Array ( [foo] =&gt; bar )
<br>&#xA0;&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$requestData</span><span class="keyword">);
<br>
<br>&#xA0;&#xA0; </span><span class="comment">// Do something with data...
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>
<br>DELETE&#xA0; can be done in exactly the same way.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are doing a POST, and the content length is 1,025 or greater, then curl exploits a feature of http 1.1: 100 (Continue) Status.<br><br>See <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8.2.3" rel="nofollow" target="_blank">http://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8.2.3</a><br><br>* it adds a header, &quot;Expect: 100-continue&quot;.&#xA0; <br>* it then sends the request head, waits for a 100 response code, then sends the content <br><br>Not all web servers support this though.&#xA0; Various errors are returned depending on the server.&#xA0; If this happens to you, suppress the &quot;Expect&quot; header with this command:<br><br><span class="default">&lt;?php<br>curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">, array(</span><span class="string">&apos;Expect:&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>See <a href="http://www.gnegg.ch/2007/02/the-return-of-except-100-continue/" rel="nofollow" target="_blank">http://www.gnegg.ch/2007/02/the-return-of-except-100-continue/</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to Curl to follow redirects and you would also like Curl to echo back any cookies that are set in the process, use this:
<br>
<br><span class="default">&lt;?php curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_COOKIEJAR</span><span class="keyword">, </span><span class="string">&apos;-&apos;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>&apos;-&apos; means stdout
<br>
<br>-dw</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you would like to send xml request to a server (lets say, making a soap proxy),
<br>you have to set
<br>
<br><span class="default">&lt;?php
<br>curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">, Array(</span><span class="string">&quot;Content-Type: text/xml&quot;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>makesure you watch for cache issue:
<br>the below code will prevent cache...
<br>
<br><span class="default">&lt;?php
<br>curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_FORBID_REUSE</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_FRESH_CONNECT</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>hope it helps ;)</span>
</div>
  

#


<div class="phpcode"><span class="html">
I spent a couple of days trying to POST a multi-dimensional array of form fields, including a file upload, to a remote server to update a product. Here are the breakthroughs that FINALLY allowed the script to run as desired.<br><br>Firstly, the HTML form used input names like these:<br>&lt;input type=&quot;text&quot; name=&quot;product[name]&quot; /&gt;<br>&lt;input type=&quot;text&quot; name=&quot;product[cost]&quot; /&gt;<br>&lt;input type=&quot;file&quot; name=&quot;product[thumbnail]&quot; /&gt;<br>in conjunction with two other form inputs not part of the product array<br>&lt;input type=&quot;text&quot; name=&quot;method&quot; value=&quot;put&quot; /&gt;<br>&lt;input type=&quot;text&quot; name=&quot;mode&quot; /&gt;<br><br>I used several cURL options, but the only two (other than URL) that mattered were:<br>curl_setopt($handle, CURLOPT_POST, true);<br>curl_setopt($handle, CURLOPT_POSTFIELDS, $postfields);<br>Pretty standard so far.<br>Note: headers didn&apos;t need to be set, cURL automatically sets headers (like content-type: multipart/form-data; content-length...) when you pass an array into CURLOPT_POSTFIELDS.<br>Note: even though this is supposed to be a PUT command through an HTTP POST form, no special PUT options needed to be passed natively through cURL. Options such as<br>curl_setopt($handle, CURLOPT_HTTPHEADER, array(&apos;X-HTTP-Method-Override: PUT&apos;, &apos;Content-Length: &apos; . strlen($fields)));<br>or<br>curl_setopt($handle, CURLOPT_PUT, true);<br>or<br>curl_setopt($handle, CURLOPT_CUSTOMREQUEST, &quot;PUT);<br>were not needed to make the code work.<br><br>The fields I wanted to pass through cURL were arranged into an array something like this:<br>$postfields = array(&quot;method&quot; =&gt; $_POST[&quot;method&quot;],<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;mode&quot; =&gt; $_POST[&quot;mode&quot;],<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;product&quot; =&gt; array(&quot;name&quot; =&gt; $_POST[&quot;product&quot;], <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;cost&quot; =&gt; $_POST[&quot;product&quot;][&quot;cost&quot;], <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;thumbnail&quot; =&gt; &quot;@{$_FILES[&quot;thumbnail&quot;][&quot;tmp_name&quot;]};type={$_FILES[&quot;thumbnail&quot;][&quot;type&quot;]}&quot;)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );<br><br>-Notice how the @ precedes the temporary filename, this creates a link so PHP will upload/transfer an actual file instead of just the file name, which would happen if the @ isn&apos;t included.<br>-Notice how I forcefully set the mime-type of the file to upload. I was having issues where images filetypes were defaulting to octet-stream instead of image/png or image/jpeg or whatever the type of the selected image.<br><br>I then tried passing $postfields straight into curl_setopt($this-&gt;handle, CURLOPT_POSTFIELDS, $postfields); but it didn&apos;t work.<br>I tried using http_build_query($postfields); but that didn&apos;t work properly either.<br>In both cases either the file wouldn&apos;t be treated as an actual file and the form data wasn&apos;t being sent properly. The problem was HTTP&apos;s methods of transmitting arrays. While PHP and other languages can figure out how to handle arrays passed via forms, HTTP isn&apos;t quite as sofisticated. I had to rewrite the $postfields array like so:<br>$postfields = array(&quot;method&quot; =&gt; $_POST[&quot;method&quot;],<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;mode&quot; =&gt; $_POST[&quot;mode&quot;],<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;product[name]&quot; =&gt; $_POST[&quot;product&quot;], <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;product[cost]&quot; =&gt; $_POST[&quot;product&quot;][&quot;cost&quot;], <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;product[thumbnail]&quot; =&gt; &quot;@{$_FILES[&quot;thumbnail&quot;][&quot;tmp_name&quot;]}&quot;);<br>curl_setopt($handle, CURLOPT_POSTFIELDS, $postfields);<br><br>This, without the use of http_build_query, solved all of my problems. Now the receiving host outputs both $_POST and $_FILES vars correctly.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want cURL to timeout in less than one second, you can use CURLOPT_TIMEOUT_MS, although there is a bug/&quot;feature&quot;&#xA0; on &quot;Unix-like systems&quot; that causes libcurl to timeout immediately if the value is &lt; 1000 ms with the error &quot;cURL Error (28): Timeout was reached&quot;.&#xA0; The explanation for this behavior is:<br><br>&quot;If libcurl is built to use the standard system name resolver, that portion of the transfer will still use full-second resolution for timeouts with a minimum timeout allowed of one second.&quot;<br><br>What this means to PHP developers is &quot;You can use this function without testing it first, because you can&apos;t tell if libcurl is using the standard system name resolver (but you can be pretty sure it is)&quot;<br><br>The problem is that on (Li|U)nix, when libcurl uses the standard name resolver, a SIGALRM is raised during name resolution which libcurl thinks is the timeout alarm.<br><br>The solution is to disable signals using CURLOPT_NOSIGNAL.&#xA0; Here&apos;s an example script that requests itself causing a 10-second delay so you can test timeouts:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (!isset(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;foo&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Client<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">(</span><span class="string">&apos;<a href="http://localhost/test/test_timeout.php?foo=bar" rel="nofollow" target="_blank">http://localhost/test/test_timeout.php?foo=bar</a>&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_NOSIGNAL</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_TIMEOUT_MS</span><span class="keyword">, </span><span class="default">200</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$curl_errno </span><span class="keyword">= </span><span class="default">curl_errno</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$curl_error </span><span class="keyword">= </span><span class="default">curl_error</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$curl_errno </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;cURL Error (</span><span class="default">$curl_errno</span><span class="string">): </span><span class="default">$curl_error</span><span class="string">\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Data received: </span><span class="default">$data</span><span class="string">\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>} else {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Server<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Done.&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you wish to find the size of the file you are streaming and use it as your header this is how:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">write_function</span><span class="keyword">(</span><span class="default">$curl_resource</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if(</span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$curl_resource</span><span class="keyword">, </span><span class="default">CURLINFO_SIZE_DOWNLOAD</span><span class="keyword">) &lt;= </span><span class="default">2000</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Expires: 0&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Cache-Control: must-revalidate, post-check=0, pre-check=0&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Pragma: public&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Description: File Transfer&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Content-Transfer-Encoding: binary&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Content-Type: &quot;</span><span class="keyword">.</span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$curl_resource</span><span class="keyword">, </span><span class="default">CURLINFO_CONTENT_TYPE</span><span class="keyword">).</span><span class="string">&quot;&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Content-Length: &quot;</span><span class="keyword">.</span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$curl_resource</span><span class="keyword">, </span><span class="default">CURLINFO_CONTENT_LENGTH_DOWNLOAD</span><span class="keyword">).</span><span class="string">&quot;&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; print </span><span class="default">$string</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; return </span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="string">&apos;8bit&apos;</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>1440 is the the default number of bytes curl will call the write function (BUFFERSIZE does not affect this, i actually think you can not change this value), so it means the headers are going to be set only one time.
<br>
<br>write_function must return the exact number of bytes of the string, so you can return a value with mb_strlen.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Many hosters use PHP safe_mode or/and open_basedir, so you can&apos;t use CURLOPT_FOLLOWLOCATION. If you try, you see message like this:<br>CURLOPT_FOLLOWLOCATION cannot be activated when safe_mode is enabled or an open_basedir is set in [you script name &amp; path] on line XXX<br><br>First, I try to use zsalab function (<a href="http://us2.php.net/manual/en/function.curl-setopt.php#102121" rel="nofollow" target="_blank">http://us2.php.net/manual/en/function.curl-setopt.php#102121</a>) from this page, but for some reason it did not work properly. So, I wrote my own.<br><br>It can be use instead of curl_exec. If server HTTP response codes is 30x, function will forward the request as long as the response is not different from 30x (for example, 200 Ok). Also you can use POST.<br><br>function curlExec(/* Array */$curlOptions=&apos;&apos;, /* Array */$curlHeaders=&apos;&apos;, /* Array */$postFields=&apos;&apos;)<br>{<br>&#xA0; $newUrl = &apos;&apos;;<br>&#xA0; $maxRedirection = 10;<br>&#xA0; do<br>&#xA0; {<br>&#xA0; &#xA0; if ($maxRedirection&lt;1) die(&apos;Error: reached the limit of redirections&apos;);<br><br>&#xA0; &#xA0; $ch = curl_init();<br>&#xA0; &#xA0; if (!empty($curlOptions)) curl_setopt_array($ch, $curlOptions);<br>&#xA0; &#xA0; if (!empty($curlHeaders)) curl_setopt($ch, CURLOPT_HTTPHEADER, $curlHeaders);<br>&#xA0; &#xA0; if (!empty($postFields))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; curl_setopt($ch, CURLOPT_POST, 1);<br>&#xA0; &#xA0; &#xA0; curl_setopt($ch, CURLOPT_POSTFIELDS, $postFields);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (!empty($newUrl)) curl_setopt($ch, CURLOPT_URL, $newUrl); // redirect needed<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; $curlResult = curl_exec($ch);<br>&#xA0; &#xA0; $code = curl_getinfo($ch, CURLINFO_HTTP_CODE);<br><br>&#xA0; &#xA0; if ($code == 301 || $code == 302 || $code == 303 || $code == 307)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; preg_match(&apos;/Location:(.*?)\n/&apos;, $curlResult, $matches);<br>&#xA0; &#xA0; &#xA0; $newUrl = trim(array_pop($matches));<br>&#xA0; &#xA0; &#xA0; curl_close($ch);<br><br>&#xA0; &#xA0; &#xA0; $maxRedirection--;<br>&#xA0; &#xA0; &#xA0; continue;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else // no more redirection<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; $code = 0;<br>&#xA0; &#xA0; &#xA0; curl_close($ch);<br>&#xA0; &#xA0; }<br>&#xA0; }<br>&#xA0; while($code);<br>&#xA0; return $curlResult;<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
When CURLOPT_FOLLOWLOCATION and CURLOPT_HEADER are both true and redirect/s have happened then the header returned by curl_exec() will contain all the headers in the redirect chain in the order they were encountered.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve found that setting CURLOPT_HTTPHEADER more than once will clear out any headers you&apos;ve set previously with CURLOPT_HTTPHEADER.<br><br>Consider the following:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment"># ...<br><br>&#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$cURL</span><span class="keyword">,</span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">,array (<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-Type: text/xml; charset=utf-8&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Expect: 100-continue&quot;<br>&#xA0; &#xA0; </span><span class="keyword">));<br><br>&#xA0; &#xA0; </span><span class="comment"># ... do some other stuff ...<br><br>&#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$cURL</span><span class="keyword">,</span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">,array (<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Accept: application/json&quot;<br>&#xA0; &#xA0; </span><span class="keyword">));<br><br>&#xA0; &#xA0; </span><span class="comment"># ...<br></span><span class="default">?&gt;<br></span><br>Both the Content-Type and Expect I set will not be in the outgoing headers, but Accept will.</span>
</div>
  

#


<div class="phpcode"><span class="html">
After much struggling, I managed to get a SOAP request requiring HTTP authentication to work.&#xA0; Here&apos;s some source that will hopefully be useful to others. 
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <span class="default">&lt;?php
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $credentials </span><span class="keyword">= </span><span class="string">&quot;username:password&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// Read the XML to send to the Web Service
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$request_file </span><span class="keyword">= </span><span class="string">&quot;./SampleRequest.xml&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fh </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$request_file</span><span class="keyword">, </span><span class="string">&apos;r&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$xml_data </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$request_file</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$url </span><span class="keyword">= </span><span class="string">&quot;<a href="http://www.example.com/services/calculation" rel="nofollow" target="_blank">http://www.example.com/services/calculation</a>&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$page </span><span class="keyword">= </span><span class="string">&quot;/services/calculation&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$headers </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;POST &quot;</span><span class="keyword">.</span><span class="default">$page</span><span class="keyword">.</span><span class="string">&quot; HTTP/1.0&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-type: text/xml;charset=\&quot;utf-8\&quot;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Accept: text/xml&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Cache-Control: no-cache&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Pragma: no-cache&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;SOAPAction: \&quot;run\&quot;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-length: &quot;</span><span class="keyword">.</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$xml_data</span><span class="keyword">),
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Authorization: Basic &quot; </span><span class="keyword">. </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$credentials</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; );
<br>&#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">,</span><span class="default">$url</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_TIMEOUT</span><span class="keyword">, </span><span class="default">60</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">, </span><span class="default">$headers</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_USERAGENT</span><span class="keyword">, </span><span class="default">$defined_vars</span><span class="keyword">[</span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Apply the XML to our curl call
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_POST</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_POSTFIELDS</span><span class="keyword">, </span><span class="default">$xml_data</span><span class="keyword">); 
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">); 
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">curl_errno</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;Error: &quot; </span><span class="keyword">. </span><span class="default">curl_error</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Show me the result
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It appears that setting CURLOPT_FILE before setting CURLOPT_RETURNTRANSFER doesn&apos;t work, presumably because CURLOPT_FILE depends on CURLOPT_RETURNTRANSFER being set.
<br>
<br>So do this:
<br>
<br><span class="default">&lt;?php
<br>curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_FILE</span><span class="keyword">, </span><span class="default">$fp</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>not this:
<br>
<br><span class="default">&lt;?php
<br>curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_FILE</span><span class="keyword">, </span><span class="default">$fp</span><span class="keyword">);
<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sometimes you can&apos;t use CURLOPT_COOKIEJAR and CURLOPT_COOKIEFILE becoz of the server php-settings(They say u may grab any files from server using these options). Here is the solution<br>1)Don&apos;t use CURLOPT_FOLLOWLOCATION<br>2)Use curl_setopt($ch, CURLOPT_HEADER, 1)<br>3)Grab from the header cookies like this:<br>preg_match_all(&apos;|Set-Cookie: (.*);|U&apos;, $content, $results);&#xA0; &#xA0; <br>$cookies = implode(&apos;;&apos;, $results[1]);<br>4)Set them using curl_setopt($ch, CURLOPT_COOKIE,&#xA0; $cookies);<br><br>Good Luck, Yevgen</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re getting trouble with cookie handling in curl: 
<br>
<br>- curl manages tranparently cookies in a single curl session
<br>- the option 
<br><span class="default">&lt;?php curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_COOKIEJAR</span><span class="keyword">, </span><span class="string">&quot;/tmp/cookieFileName&quot;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>makes curl to store the cookies in a file at the and of the curl session
<br>
<br>- the option 
<br><span class="default">&lt;?php curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_COOKIEFILE</span><span class="keyword">, </span><span class="string">&quot;/tmp/cookieFileName&quot;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>makes curl to use the given file as source for the cookies to send to the server. 
<br>
<br>so to handle correctly cookies between different curl session, the you have to do something like this:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; &#xA0;&#xA0; $ch </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">(); 
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">curl_setopt </span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$url</span><span class="keyword">); 
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">curl_setopt </span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_COOKIEJAR</span><span class="keyword">, </span><span class="default">COOKIE_FILE_PATH</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">curl_setopt </span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_COOKIEFILE</span><span class="keyword">, </span><span class="default">COOKIE_FILE_PATH</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">curl_setopt </span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">); 
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">curl_exec </span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>in particular this is NECESSARY if you are using PEAR_SOAP libraries to build a webservice client over https and the remote server need to establish a session cookie. in fact each soap message is sent using a different curl session!!
<br>
<br>I hope this can help someone
<br>Luca</span>
</div>
  

#


<div class="phpcode"><span class="html">
Handling redirections with curl if safe_mode or open_basedir is enabled. The function working transparent, no problem with header and returntransfer options. You can handle the max redirection with the optional second argument (the function is set the variable to zero if max redirection exceeded).
<br>Second parameter values:
<br>- maxredirect is null or not set: redirect maximum five time, after raise PHP warning
<br>- maxredirect is greather then zero: no raiser error, but parameter variable set to zero
<br>- maxredirect is less or equal zero: no follow redirections
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">curl_exec_follow</span><span class="keyword">(</span><span class="comment">/*resource*/ </span><span class="default">$ch</span><span class="keyword">, </span><span class="comment">/*int*/ </span><span class="keyword">&amp;</span><span class="default">$maxredirect </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$mr </span><span class="keyword">= </span><span class="default">$maxredirect </span><span class="keyword">=== </span><span class="default">null </span><span class="keyword">? </span><span class="default">5 </span><span class="keyword">: </span><span class="default">intval</span><span class="keyword">(</span><span class="default">$maxredirect</span><span class="keyword">);
<br>&#xA0; &#xA0; if (</span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&apos;open_basedir&apos;</span><span class="keyword">) == </span><span class="string">&apos;&apos; </span><span class="keyword">&amp;&amp; </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&apos;safe_mode&apos; </span><span class="keyword">== </span><span class="string">&apos;Off&apos;</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_FOLLOWLOCATION</span><span class="keyword">, </span><span class="default">$mr </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_MAXREDIRS</span><span class="keyword">, </span><span class="default">$mr</span><span class="keyword">);
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_FOLLOWLOCATION</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$mr </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newurl </span><span class="keyword">= </span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLINFO_EFFECTIVE_URL</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rch </span><span class="keyword">= </span><span class="default">curl_copy_handle</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">, </span><span class="default">CURLOPT_HEADER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">, </span><span class="default">CURLOPT_NOBODY</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">, </span><span class="default">CURLOPT_FORBID_REUSE</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; do {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$newurl</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$header </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">curl_errno</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$code </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$code </span><span class="keyword">= </span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">, </span><span class="default">CURLINFO_HTTP_CODE</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$code </span><span class="keyword">== </span><span class="default">301 </span><span class="keyword">|| </span><span class="default">$code </span><span class="keyword">== </span><span class="default">302</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/Location:(.*?)\n/&apos;</span><span class="keyword">, </span><span class="default">$header</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newurl </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$code </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } while (</span><span class="default">$code </span><span class="keyword">&amp;&amp; --</span><span class="default">$mr</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$rch</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$mr</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$maxredirect </span><span class="keyword">=== </span><span class="default">null</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="string">&apos;Too many redirects. When following redirects, libcurl hit the maximum amount.&apos;</span><span class="keyword">, </span><span class="default">E_USER_WARNING</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$maxredirect </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$newurl</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
CURLOPT_POST must be left unset if you want the Content-Type header set to &quot;multipart/form-data&quot; (e.g., when CURLOPT_POSTFIELDS is an array). If you set CURLOPT_POSTFIELDS to an array and have CURLOPT_POST set to TRUE, Content-Length will be -1 and most sane servers will reject the request. If you set CURLOPT_POSTFIELDS to an array and have CURLOPT_POST set to FALSE, cURL will send a GET request.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Passing in PHP&apos;s $_SESSION into your cURL call:
<br>
<br><span class="default">&lt;?php
<br>session_start</span><span class="keyword">();
<br></span><span class="default">$strCookie </span><span class="keyword">= </span><span class="string">&apos;PHPSESSID=&apos; </span><span class="keyword">. </span><span class="default">$_COOKIE</span><span class="keyword">[</span><span class="string">&apos;PHPSESSID&apos;</span><span class="keyword">] . </span><span class="string">&apos;; path=/&apos;</span><span class="keyword">;
<br></span><span class="default">session_write_close</span><span class="keyword">();
<br>
<br></span><span class="default">$curl_handle </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">(</span><span class="string">&apos;enter_external_url_here&apos;</span><span class="keyword">);
<br></span><span class="default">curl_setopt</span><span class="keyword">( </span><span class="default">$curl_handle</span><span class="keyword">, </span><span class="default">CURLOPT_COOKIE</span><span class="keyword">, </span><span class="default">$strCookie </span><span class="keyword">);
<br></span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$curl_handle</span><span class="keyword">);
<br></span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$curl_handle</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>This worked great for me.&#xA0; I was calling pages from the same server and needed to keep the $_SESSION variables.&#xA0; This passes them over.&#xA0; If you want to test, just print_r($_SESSION);
<br>
<br>Enjoy!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-setopt.php)

**[⬆ to root](/)**