# getimagesizefromstring




<div class="phpcode"><span class="html">
getimagesizefromstring function for &lt; 5.4<br><br><span class="default">&lt;?php<br>&#xA0;&#xA0; </span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;getimagesizefromstring&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; function </span><span class="default">getimagesizefromstring</span><span class="keyword">(</span><span class="default">$string_data</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$uri </span><span class="keyword">= </span><span class="string">&apos;data://application/octet-stream;base64,&apos;&#xA0; </span><span class="keyword">. </span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$string_data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">getimagesize</span><span class="keyword">(</span><span class="default">$uri</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.getimagesizefromstring.php)

**[To root](/README.md)**