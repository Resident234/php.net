# zlib://




<div class="phpcode"><span class="html">
One-liners to gzip and ungzip a file:<br><br>copy(&apos;file.txt&apos;, &apos;compress.zlib://&apos; . &apos;file.txt.gz&apos;);<br><br>copy(&apos;compress.zlib://&apos; . &apos;file.txt.gz&apos;, &apos;file.txt&apos;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
Example on how to read an entry from a ZIP archive (file &quot;bar.txt&quot; inside &quot;./foo.zip&quot;):
<br>
<br><span class="default">&lt;?php
<br>
<br>$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;zip://./foo.zip#bar.txt&apos;</span><span class="keyword">, </span><span class="string">&apos;r&apos;</span><span class="keyword">);
<br>if( </span><span class="default">$fp </span><span class="keyword">){
<br>&#xA0; &#xA0; while( !</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">) ){
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">8192</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Also, apparently, the &quot;zip:&quot; wrapper does not allow writing as of PHP/5.3.6. You can read <a href="http://php.net/ziparchive-getstream" rel="nofollow" target="_blank">http://php.net/ziparchive-getstream</a> for further reference since the underlying code is probably the same.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/wrappers.compression.php)

**[⬆ to root](/)**