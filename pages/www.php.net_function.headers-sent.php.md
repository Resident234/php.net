# headers_sent




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">redirect</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">) {<br>&#xA0; &#xA0; if (!</span><span class="default">headers_sent</span><span class="keyword">())<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Location: &apos;</span><span class="keyword">.</span><span class="default">$filename</span><span class="keyword">);<br>&#xA0; &#xA0; else {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;script type=&quot;text/javascript&quot;&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;window.location.href=&quot;&apos;</span><span class="keyword">.</span><span class="default">$filename</span><span class="keyword">.</span><span class="string">&apos;&quot;;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;/script&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;noscript&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;meta http-equiv=&quot;refresh&quot; content=&quot;0;url=&apos;</span><span class="keyword">.</span><span class="default">$filename</span><span class="keyword">.</span><span class="string">&apos;&quot; /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;/noscript&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">redirect</span><span class="keyword">(</span><span class="string">&apos;<a href="http://www.google.com" rel="nofollow" target="_blank">http://www.google.com</a>&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.headers-sent.php)

**[To root](/README.md)**