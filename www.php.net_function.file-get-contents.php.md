# file_get_contents




<div class="phpcode"><span class="html">
Setting the timeout properly without messing with ini values:
<br>
<br><span class="default">&lt;?php
<br>$ctx </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;http&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;timeout&apos; </span><span class="keyword">=&gt; </span><span class="default">1
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)
<br>&#xA0; &#xA0; )
<br>);
<br></span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&quot;<a href="http://example.com/" rel="nofollow" target="_blank">http://example.com/</a>&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$ctx</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
file_get_contents can do a POST, create a context for that first:<br><br>$opts = array(&apos;http&apos; =&gt;<br>&#xA0; array(<br>&#xA0; &#xA0; &apos;method&apos;&#xA0; =&gt; &apos;POST&apos;,<br>&#xA0; &#xA0; &apos;header&apos;&#xA0; =&gt; &quot;Content-Type: text/xml\r\n&quot;.<br>&#xA0; &#xA0; &#xA0; &quot;Authorization: Basic &quot;.base64_encode(&quot;$https_user:$https_password&quot;).&quot;\r\n&quot;,<br>&#xA0; &#xA0; &apos;content&apos; =&gt; $body,<br>&#xA0; &#xA0; &apos;timeout&apos; =&gt; 60<br>&#xA0; )<br>);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>$context&#xA0; = stream_context_create($opts);<br>$url = &apos;<a href="https://" rel="nofollow" target="_blank">https://</a>&apos;.$https_server;<br>$result = file_get_contents($url, false, $context, -1, 40000);</span>
</div>
  

#


<div class="phpcode"><span class="html">
here is another (maybe the easiest) way of doing POST http requests from php using its built-in capabilities. feel free to add the headers you need (notably the Host: header) to further customize the request.<br><br>note: this method does not allow file uploads. if you want to upload a file with your request you will need to modify the context parameters to provide multipart/form-data encoding (check out <a href="http://www.php.net/manual/en/context.http.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/context.http.php</a> ) and build the $data_url following the guidelines on <a href="http://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2" rel="nofollow" target="_blank">http://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2</a><br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br>make an http POST request and return the response content and headers<br>@param string $url&#xA0; &#xA0; url of the requested script<br>@param array $data&#xA0; &#xA0; hash array of request variables<br>@return returns a hash array with response content and headers in the following form:<br>&#xA0; &#xA0; array (&apos;content&apos;=&gt;&apos;&lt;html&gt;&lt;/html&gt;&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; , &apos;headers&apos;=&gt;array (&apos;HTTP/1.1 200 OK&apos;, &apos;Connection: close&apos;, ...)<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>*/<br></span><span class="keyword">function </span><span class="default">http_post </span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$data_url </span><span class="keyword">= </span><span class="default">http_build_query </span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$data_len </span><span class="keyword">= </span><span class="default">strlen </span><span class="keyword">(</span><span class="default">$data_url</span><span class="keyword">);<br><br>&#xA0; &#xA0; return array (</span><span class="string">&apos;content&apos;</span><span class="keyword">=&gt;</span><span class="default">file_get_contents </span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">stream_context_create </span><span class="keyword">(array (</span><span class="string">&apos;http&apos;</span><span class="keyword">=&gt;array (</span><span class="string">&apos;method&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;POST&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">, </span><span class="string">&apos;header&apos;</span><span class="keyword">=&gt;</span><span class="string">&quot;Connection: close\r\nContent-Length: </span><span class="default">$data_len</span><span class="string">\r\n&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">, </span><span class="string">&apos;content&apos;</span><span class="keyword">=&gt;</span><span class="default">$data_url<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">))))<br>&#xA0; &#xA0; &#xA0; &#xA0; , </span><span class="string">&apos;headers&apos;</span><span class="keyword">=&gt;</span><span class="default">$http_response_header<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Keep in mind that if you use a URL as the filename attribute, and the external resource is not reachable, the function will not return FALSE but instead an exception will be thrown.<br><br>So, in this case, instead of doing this:<br><br>$content = file_get_contents(&apos;<a href="https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg" rel="nofollow" target="_blank">https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg</a>&apos;);<br><br>if ($content === false) {<br>&#xA0; &#xA0; // Handle the error<br>}<br><br>Do this:<br><br>try {<br>&#xA0; &#xA0; $content = file_get_contents(&apos;<a href="https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg" rel="nofollow" target="_blank">https://en.wikipedia.org/wiki/Cat#/media/File:Large_Siamese_cat_tosses_a_mouse.jpg</a>&apos;);<br><br>&#xA0; &#xA0; if ($content === false) {<br>&#xA0; &#xA0; &#xA0; &#xA0; // Handle the error<br>&#xA0; &#xA0; }<br>} catch (Exception $e) {<br>&#xA0; &#xA0; // Handle exception<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
A UTF-8 issue I&apos;ve encountered is that of reading a URL with a non-UTF-8 encoding that is later displayed improperly since file_get_contents() related to it as UTF-8. This small function should show you how to address this issue:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">file_get_contents_utf8</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">) {
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$fn</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">mb_convert_encoding</span><span class="keyword">(</span><span class="default">$content</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mb_detect_encoding</span><span class="keyword">(</span><span class="default">$content</span><span class="keyword">, </span><span class="string">&apos;UTF-8, ISO-8859-1&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">));
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is important to write the method in capital letters like &quot;GET&quot; or &quot;POST&quot; and not &quot;get&quot; or &quot;post&quot;. Some servers can respond a 400 error if you do not use caps in the method.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Seems file looks for the file inside the current working (executing) directory before looking in the include path, even with the FILE_USE_INCLUDE_PATH flag specified.<br><br>Same behavior as include actually.<br><br>By the way I feel the doc is not entirely clear on the exact order of inclusion (see include). It seems to say the include_path is the first location to be searched, but I have come across at least one case where the directory containing the file including was actually the first to be searched.<br><br>Drat.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The offset is 0 based.&#xA0; Setting it to 1 will skip the first character of the stream.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is a nice and simple substitute to get_file_contents() using curl, it returns FALSE if $contents is empty.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">curl_get_file_contents</span><span class="keyword">(</span><span class="default">$URL</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$URL</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$contents </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$contents</span><span class="keyword">) return </span><span class="default">$contents</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else return </span><span class="default">FALSE</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Hope this help, if there is something wrong or something you don&apos;t understand let me know :)</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re having problems with binary and hex data:<br><br>I had a problem when trying to read information from a ttf, which is primarily hex data. A binary-safe file read automatically replaces byte values with their corresponding ASCII characters, so I thought that I could use the binary string when I needed readable ASCII strings, and bin2hex() when I needed hex strings.<br><br>However, this became a problem when I tried to pass those ASCII strings into other functions (namely gd functions). var_dump showed that a 5-character string contained 10 characters, but they weren&apos;t visible. A binary-to-&quot;normal&quot; string conversion function didn&apos;t seem to exist and I didn&apos;t want to have to convert every single character in hex using chr().<br><br>I used unpack with &quot;c*&quot; as the format flag to see what was going on, and found that every other character was null data (ordinal 0). To solve it, I just did<br><br>str_replace(chr(0), &quot;&quot;, $string);<br><br>which did the trick.<br><br>This took forever to figure out so I hope this helps people reading from hex data!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.file-get-contents.php)

**[To root](/README.md)**