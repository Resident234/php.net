# ftruncate




<div class="phpcode"><span class="html">
If you want to empty a file of it&apos;s contents bare in mind that opening a file in w mode truncates the file automatically, so instead of doing...<br><br><span class="default">&lt;?php<br>$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;/tmp/file.txt&quot;</span><span class="keyword">, </span><span class="string">&quot;r+&quot;</span><span class="keyword">);<br></span><span class="default">ftruncate</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>You can just do...<br><br><span class="default">&lt;?php<br>$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;/tmp/file.txt&quot;</span><span class="keyword">, </span><span class="string">&quot;w&quot;</span><span class="keyword">);<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You MUST use rewind() after ftruncate() to replace file content</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftruncate.php)

**[⬆ to root](/)**