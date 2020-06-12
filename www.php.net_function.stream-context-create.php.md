# stream_context_create




<div class="phpcode"><span class="html">
Something to keep in mind when creating SSL streams (using <a href="https://" rel="nofollow" target="_blank">https://</a>):<br><br><span class="default">&lt;?php<br>$context </span><span class="keyword">= </span><span class="default">context_create_stream</span><span class="keyword">(</span><span class="default">$context_options</span><span class="keyword">)<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;<a href="https://url" rel="nofollow" target="_blank">https://url</a>&apos;</span><span class="keyword">, </span><span class="string">&apos;r&apos;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">$context</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>One would think - the proper way to create a stream options array, would be as follows: <br><br><span class="default">&lt;?php<br>$context_options </span><span class="keyword">= array (<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;https&apos; </span><span class="keyword">=&gt; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;method&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;POST&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;header&apos;</span><span class="keyword">=&gt; </span><span class="string">&quot;Content-type: application/x-www-form-urlencoded\r\n&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Length: &quot; </span><span class="keyword">. </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) . </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;content&apos; </span><span class="keyword">=&gt; </span><span class="default">$data<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; );<br></span><span class="default">?&gt;<br></span><br>THAT IS THE WRONG WAY!!!<br>Take notice to the 3rd line: &apos;https&apos; =&gt; array (<br><br>The CORRECT way, is as follows:<br><br><span class="default">&lt;?php<br>$context_options </span><span class="keyword">= array (<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;http&apos; </span><span class="keyword">=&gt; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;method&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;POST&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;header&apos;</span><span class="keyword">=&gt; </span><span class="string">&quot;Content-type: application/x-www-form-urlencoded\r\n&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Length: &quot; </span><span class="keyword">. </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) . </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;content&apos; </span><span class="keyword">=&gt; </span><span class="default">$data<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; );<br></span><span class="default">?&gt;<br></span><br>Notice, the NEW 3rd line: &apos;http&apos; =&gt; array (<br><br>Now - keep this in mind - I spent several hours trying to trouble shoot my issue, when I finally stumbled upon this non-documented issue.<br><br>The complete code to post to a secure page is as follows:<br><br><span class="default">&lt;?php<br>$data </span><span class="keyword">= array (</span><span class="string">&apos;foo&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;bar&apos;</span><span class="keyword">, </span><span class="string">&apos;bar&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;baz&apos;</span><span class="keyword">);<br></span><span class="default">$data </span><span class="keyword">= </span><span class="default">http_build_query</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br><br></span><span class="default">$context_options </span><span class="keyword">= array (<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;http&apos; </span><span class="keyword">=&gt; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;method&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;POST&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;header&apos;</span><span class="keyword">=&gt; </span><span class="string">&quot;Content-type: application/x-www-form-urlencoded\r\n&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Length: &quot; </span><span class="keyword">. </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) . </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;content&apos; </span><span class="keyword">=&gt; </span><span class="default">$data<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; );<br><br></span><span class="default">$context </span><span class="keyword">= </span><span class="default">context_create_stream</span><span class="keyword">(</span><span class="default">$context_options</span><span class="keyword">)<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;<a href="https://url" rel="nofollow" target="_blank">https://url</a>&apos;</span><span class="keyword">, </span><span class="string">&apos;r&apos;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">$context</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I big NOTE that i hope will help some one. Something that is not mentioned in the documentation, is that when php is compiled --with-curlwrappers,
<br>
<br>So, instead of:
<br>
<br><span class="default">&lt;?php
<br>$opts </span><span class="keyword">= array(
<br>&#xA0; </span><span class="string">&apos;http&apos;</span><span class="keyword">=&gt;array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;method&apos;</span><span class="keyword">=&gt;</span><span class="string">&quot;GET&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;header&apos;</span><span class="keyword">=&gt;</span><span class="string">&quot;Accept-language: en\r\n&quot; </span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Cookie: foo=bar\r\n&quot;
<br>&#xA0; </span><span class="keyword">)
<br>);
<br>
<br></span><span class="default">$context </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$opts</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>You would setup the header this way: 
<br>
<br><span class="default">&lt;?php
<br>$opts </span><span class="keyword">= array(
<br>&#xA0; </span><span class="string">&apos;http&apos;</span><span class="keyword">=&gt;array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;method&apos;</span><span class="keyword">=&gt;</span><span class="string">&quot;GET&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;header&apos;</span><span class="keyword">=&gt;array(</span><span class="string">&quot;Accept-language: en&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;Cookie: foo=bar&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;Custom-Header: value&quot;</span><span class="keyword">)
<br>&#xA0; )
<br>);
<br>
<br></span><span class="default">$context </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$opts</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>This will work.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I spent a good five hours trying to figure this out, so hopefully it will save someone else some time.
<br>
<br>When you are trying to download a file via ftp through an HTTP proxy note that the following will not be enough:
<br><span class="default">&lt;?php
<br>$opts </span><span class="keyword">= array(</span><span class="string">&apos;ftp&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;proxy&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;tcp://vbinprst10:8080&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;request_fulluri&apos;</span><span class="keyword">=&gt;</span><span class="default">true</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;header&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Proxy-Authorization: Basic </span><span class="default">$auth</span><span class="string">&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)
<br>&#xA0; &#xA0; )
<br>);
<br></span><span class="default">$context </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$opts</span><span class="keyword">);
<br></span><span class="default">$s </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&quot;<a href="ftp://anonymous:anonymous@ftp.example.org" rel="nofollow" target="_blank">ftp://anonymous:anonymous@ftp.example.org</a>&quot;</span><span class="keyword">,</span><span class="default">false</span><span class="keyword">,</span><span class="default">$context</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Your proxy will respond that authentication is required. You may scratch your head and think &quot;but I&apos;m providing authentication!&quot;
<br>
<br>The issue is that the &apos;header&apos; value is only applicable to http connections. So to authenticate on a proxy, you first have to pull a file from HTTP, before the context is valid for using on FTP.
<br><span class="default">&lt;?php
<br>$opts </span><span class="keyword">= array(</span><span class="string">&apos;ftp&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;proxy&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;tcp://vbinprst10:8080&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;request_fulluri&apos;</span><span class="keyword">=&gt;</span><span class="default">true</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;header&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Proxy-Authorization: Basic </span><span class="default">$auth</span><span class="string">&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)
<br>&#xA0; &#xA0; ),
<br>&#xA0; &#xA0; </span><span class="string">&apos;http&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;proxy&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;tcp://vbinprst10:8080&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;request_fulluri&apos;</span><span class="keyword">=&gt;</span><span class="default">true</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;header&apos; </span><span class="keyword">=&gt; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Proxy-Authorization: Basic </span><span class="default">$auth</span><span class="string">&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)
<br>&#xA0; &#xA0; )
<br>);
<br></span><span class="default">$context </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$opts</span><span class="keyword">);
<br></span><span class="default">$s </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&quot;<a href="http://www.example.com" rel="nofollow" target="_blank">http://www.example.com</a>&quot;</span><span class="keyword">,</span><span class="default">false</span><span class="keyword">,</span><span class="default">$context</span><span class="keyword">);
<br></span><span class="default">$s </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&quot;<a href="ftp://anonymous:anonymous@ftp.example.org" rel="nofollow" target="_blank">ftp://anonymous:anonymous@ftp.example.org</a>&quot;</span><span class="keyword">,</span><span class="default">false</span><span class="keyword">,</span><span class="default">$context</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>It&apos;s a bit roundabout, but it works. Note that the &apos;header&apos; val in the ftp array is redundant, but I kept it in anyway.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I found the following code worked for me for POSTing some binary data to a remote server. I am putting it here since I could not find a quick solution to this by &apos;googling&apos; or looking through this documentation. <br><br>Disclaimer:&#xA0; I have no idea if this a &apos;good&apos; solution, since I&apos;m new to PHP, but it may just suit your needs as it did mine.&#xA0; I am assuming bad things will happen with very large files since the entire file is read into $fileContents. <br><br>I am using PHP 5.2.8.<br><br>&#xA0;&#xA0; $fileHandle = fopen(&quot;someImage.jpg&quot;, &quot;rb&quot;);<br>&#xA0;&#xA0; $fileContents = stream_get_contents($fileHandle);<br>&#xA0;&#xA0; fclose($fileHandle);<br><br>&#xA0;&#xA0; $params = array(<br>&#xA0; &#xA0; &#xA0; &apos;http&apos; =&gt; array<br>&#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;method&apos; =&gt; &apos;POST&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;header&apos;=&gt;&quot;Content-Type: multipart/form-data\r\n&quot;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;content&apos; =&gt; $fileContents<br>&#xA0; &#xA0; &#xA0; )<br>&#xA0;&#xA0; );<br>&#xA0;&#xA0; $url = &quot;<a href="http://somesite.somecompany.com?someParam=someValue" rel="nofollow" target="_blank">http://somesite.somecompany.com?someParam=someValue</a>&quot;;<br>&#xA0;&#xA0; $ctx = stream_context_create($params);<br>&#xA0;&#xA0; $fp = fopen($url, &apos;rb&apos;, false, $ctx);<br><br>&#xA0;&#xA0; $response = stream_get_contents($fp);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-context-create.php)

**[⬆ to root](/)**