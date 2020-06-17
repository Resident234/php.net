# The CURLFile class




<div class="phpcode"><span class="html">
There are &quot;@&quot; issue on multipart POST requests.<br><br>Solution for PHP 5.5 or later:<br>- Enable CURLOPT_SAFE_UPLOAD.<br>- Use CURLFile instead of &quot;@&quot;.<br><br>Solution for PHP 5.4 or earlier:<br>- Build up multipart content body by youself.<br>- Change &quot;Content-Type&quot; header by yourself.<br><br>The following snippet will help you :D<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * For safe multipart POST request for PHP5.3 ~ PHP 5.4.<br> * <br> * @param resource $ch cURL resource<br> * @param array $assoc &quot;name =&gt; value&quot;<br> * @param array $files &quot;name =&gt; path&quot;<br> * @return bool<br> */<br></span><span class="keyword">function </span><span class="default">curl_custom_postfields</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, array </span><span class="default">$assoc </span><span class="keyword">= array(), array </span><span class="default">$files </span><span class="keyword">= array()) {<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// invalid characters for &quot;name&quot; and &quot;filename&quot;<br>&#xA0; &#xA0; </span><span class="keyword">static </span><span class="default">$disallow </span><span class="keyword">= array(</span><span class="string">&quot;\0&quot;</span><span class="keyword">, </span><span class="string">&quot;\&quot;&quot;</span><span class="keyword">, </span><span class="string">&quot;\r&quot;</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// build normal parameters<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$assoc </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$k </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$disallow</span><span class="keyword">, </span><span class="string">&quot;_&quot;</span><span class="keyword">, </span><span class="default">$k</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$body</span><span class="keyword">[] = </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">, array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-Disposition: form-data; name=\&quot;</span><span class="keyword">{</span><span class="default">$k</span><span class="keyword">}</span><span class="string">\&quot;&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">), <br>&#xA0; &#xA0; &#xA0; &#xA0; ));<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// build file parameters<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$files </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; switch (</span><span class="default">true</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">false </span><span class="keyword">=== </span><span class="default">$v </span><span class="keyword">= </span><span class="default">realpath</span><span class="keyword">(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">)):<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case !</span><span class="default">is_file</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">):<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case !</span><span class="default">is_readable</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">):<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue; </span><span class="comment">// or return false, throw new InvalidArgumentException<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$v </span><span class="keyword">= </span><span class="default">call_user_func</span><span class="keyword">(</span><span class="string">&quot;end&quot;</span><span class="keyword">, </span><span class="default">explode</span><span class="keyword">(</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">, </span><span class="default">$v</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$k </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$disallow</span><span class="keyword">, </span><span class="string">&quot;_&quot;</span><span class="keyword">, </span><span class="default">$k</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$v </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$disallow</span><span class="keyword">, </span><span class="string">&quot;_&quot;</span><span class="keyword">, </span><span class="default">$v</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$body</span><span class="keyword">[] = </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">, array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-Disposition: form-data; name=\&quot;</span><span class="keyword">{</span><span class="default">$k</span><span class="keyword">}</span><span class="string">\&quot;; filename=\&quot;</span><span class="keyword">{</span><span class="default">$v</span><span class="keyword">}</span><span class="string">\&quot;&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-Type: application/octet-stream&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data</span><span class="keyword">, <br>&#xA0; &#xA0; &#xA0; &#xA0; ));<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// generate safe boundary <br>&#xA0; &#xA0; </span><span class="keyword">do {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$boundary </span><span class="keyword">= </span><span class="string">&quot;---------------------&quot; </span><span class="keyword">. </span><span class="default">md5</span><span class="keyword">(</span><span class="default">mt_rand</span><span class="keyword">() . </span><span class="default">microtime</span><span class="keyword">());<br>&#xA0; &#xA0; } while (</span><span class="default">preg_grep</span><span class="keyword">(</span><span class="string">&quot;/</span><span class="keyword">{</span><span class="default">$boundary</span><span class="keyword">}</span><span class="string">/&quot;</span><span class="keyword">, </span><span class="default">$body</span><span class="keyword">));<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// add boundary for each parameters<br>&#xA0; &#xA0; </span><span class="default">array_walk</span><span class="keyword">(</span><span class="default">$body</span><span class="keyword">, function (&amp;</span><span class="default">$part</span><span class="keyword">) use (</span><span class="default">$boundary</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$part </span><span class="keyword">= </span><span class="string">&quot;--</span><span class="keyword">{</span><span class="default">$boundary</span><span class="keyword">}</span><span class="string">\r\n</span><span class="keyword">{</span><span class="default">$part</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; });<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// add final boundary<br>&#xA0; &#xA0; </span><span class="default">$body</span><span class="keyword">[] = </span><span class="string">&quot;--</span><span class="keyword">{</span><span class="default">$boundary</span><span class="keyword">}</span><span class="string">--&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$body</span><span class="keyword">[] = </span><span class="string">&quot;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// set options<br>&#xA0; &#xA0; </span><span class="keyword">return @</span><span class="default">curl_setopt_array</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_POST&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_POSTFIELDS </span><span class="keyword">=&gt; </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">, </span><span class="default">$body</span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">CURLOPT_HTTPHEADER </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Expect: 100-continue&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;Content-Type: multipart/form-data; boundary=</span><span class="keyword">{</span><span class="default">$boundary</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">, </span><span class="comment">// change Content-Type<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">),<br>&#xA0; &#xA0; ));<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
i&apos;ve seen some downvotes , here is a small example of using curl to upload image .<br><br><span class="default">&lt;?php<br>$target</span><span class="keyword">=</span><span class="string">&quot;<a href="http://youraddress.tld/example/upload.php" rel="nofollow" target="_blank">http://youraddress.tld/example/upload.php</a>&quot;</span><span class="keyword">;<br><br></span><span class="comment"># <a href="http://php.net/manual/en/curlfile.construct.php" rel="nofollow" target="_blank">http://php.net/manual/en/curlfile.construct.php</a><br><br>// Create a CURLFile object / procedural method <br></span><span class="default">$cfile </span><span class="keyword">= </span><span class="default">curl_file_create</span><span class="keyword">(</span><span class="string">&apos;resource/test.png&apos;</span><span class="keyword">,</span><span class="string">&apos;image/png&apos;</span><span class="keyword">,</span><span class="string">&apos;testpic&apos;</span><span class="keyword">); </span><span class="comment">// try adding <br><br>// Create a CURLFile object / oop method <br>#$cfile = new CURLFile(&apos;resource/test.png&apos;,&apos;image/png&apos;,&apos;testpic&apos;); // uncomment and use if the upper procedural method is not working.<br><br>// Assign POST data<br></span><span class="default">$imgdata </span><span class="keyword">= array(</span><span class="string">&apos;myimage&apos; </span><span class="keyword">=&gt; </span><span class="default">$cfile</span><span class="keyword">);<br><br></span><span class="default">$curl </span><span class="keyword">= </span><span class="default">curl_init</span><span class="keyword">();<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$target</span><span class="keyword">);<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_USERAGENT</span><span class="keyword">,</span><span class="string">&apos;Opera/9.80 (Windows NT 6.2; Win64; x64) Presto/2.12.388 Version/12.15&apos;</span><span class="keyword">);<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_HTTPHEADER</span><span class="keyword">,array(</span><span class="string">&apos;User-Agent: Opera/9.80 (Windows NT 6.2; Win64; x64) Presto/2.12.388 Version/12.15&apos;</span><span class="keyword">,</span><span class="string">&apos;Referer: <a href="http://someaddress.tld" rel="nofollow" target="_blank">http://someaddress.tld</a>&apos;</span><span class="keyword">,</span><span class="string">&apos;Content-Type: multipart/form-data&apos;</span><span class="keyword">));<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_SSL_VERIFYPEER</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">); </span><span class="comment">// stop verifying certificate<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); <br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_POST</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// enable posting<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_POSTFIELDS</span><span class="keyword">, </span><span class="default">$imgdata</span><span class="keyword">); </span><span class="comment">// post images <br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">, </span><span class="default">CURLOPT_FOLLOWLOCATION</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); </span><span class="comment">// if any redirection after upload<br></span><span class="default">$r </span><span class="keyword">= </span><span class="default">curl_exec</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">); <br></span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$curl</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.curlfile.php)

**[To root](/README.md)**