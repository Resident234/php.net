# Examples




<div class="phpcode"><span class="html">
All these examples will not work if the php script has no write access within the folder. <br><br>Although you may say this is obvious, I found that in this case, $zip-&gt;open(&quot;name&quot;, ZIPARCHIVE::CREATE) doesn&apos;t return an error as it might not try to access the file system but rather allocates memory. <br><br>It is only $zip-&gt;close() that returns the error. This might cause you seeking at the wrong end.</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php
<br>&#xA0; &#xA0; &#xA0; &#xA0; $zip </span><span class="keyword">= new </span><span class="default">ZipArchive</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">open</span><span class="keyword">(</span><span class="string">&apos;teste.zip&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">extractTo</span><span class="keyword">(</span><span class="string">&apos;./&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Ok!&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/zip.examples.php)

**[To root](/README.md)**