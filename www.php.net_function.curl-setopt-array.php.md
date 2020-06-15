# curl_setopt_array




<div class="phpcode"><span class="html">
In case that you need to read SSL page content from https with curl, this function can help you:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">get_web_page</span><span class="keyword">( </span><span class="default">$url</span><span class="keyword">,</span><span class="default">$curl_data </span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$options </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_RETURNTRANSFER </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// return web page
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_HEADER&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">false</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// don&apos;t return headers
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_FOLLOWLOCATION </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// follow redirects
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_ENCODING&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;&quot;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// handle all encodings
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_USERAGENT&#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="string">&quot;spider&quot;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="comment">// who am i
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_AUTOREFERER&#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// set referer on redirect
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_CONNECTTIMEOUT </span><span class="keyword">=&gt; </span><span class="default">120</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// timeout on connect
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_TIMEOUT&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">120</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// timeout on response
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_MAXREDIRS&#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">10</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// stop after 10 redirects
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_POST&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// i am sending post data
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">CURLOPT_POSTFIELDS&#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">$curl_data</span><span class="keyword">,&#xA0; &#xA0; </span><span class="comment">// this are my post vars
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_SSL_VERIFYHOST </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// don&apos;t verify ssl
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_SSL_VERIFYPEER </span><span class="keyword">=&gt; </span><span class="default">false</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_VERBOSE&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">1&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//
<br>&#xA0; &#xA0; </span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$ch&#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">curl_setopt_array</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">,</span><span class="default">$options</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$err&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">curl_errno</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$errmsg&#xA0; </span><span class="keyword">= </span><span class="default">curl_error</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">) ;
<br>&#xA0; &#xA0; </span><span class="default">$header&#xA0; </span><span class="keyword">= </span><span class="default">curl_getinfo</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>
<br>&#xA0; </span><span class="comment">//&#xA0; $header[&apos;errno&apos;]&#xA0;&#xA0; = $err;
<br>&#xA0; //&#xA0; $header[&apos;errmsg&apos;]&#xA0; = $errmsg;
<br>&#xA0; //&#xA0; $header[&apos;content&apos;] = $content;
<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$header</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">$curl_data </span><span class="keyword">= </span><span class="string">&quot;var1=60&amp;var2=test&quot;</span><span class="keyword">;
<br></span><span class="default">$url </span><span class="keyword">= </span><span class="string">&quot;<a href="https://www.example.com" rel="nofollow" target="_blank">https://www.example.com</a>&quot;</span><span class="keyword">;
<br></span><span class="default">$response </span><span class="keyword">= </span><span class="default">get_web_page</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">,</span><span class="default">$curl_data</span><span class="keyword">);
<br>
<br>print </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$response</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.curl-setopt-array.php)

**[To root](/README.md)**