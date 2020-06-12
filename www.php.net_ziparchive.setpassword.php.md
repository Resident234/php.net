# ZipArchive::setPassword




<div class="phpcode"><span class="html">
It seems that this function supports only decryption of password protected archives (see changelog: <a href="http://pecl.php.net/package-changelog.php?package=zip" rel="nofollow" target="_blank">http://pecl.php.net/package-changelog.php?package=zip</a>). Creation of password protected archives is not supported (they will be created simply as non-protected archives).<br><br>Example code for extraction of files from password protected ZIP archives:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $zip </span><span class="keyword">= new </span><span class="default">ZipArchive</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$zip_status </span><span class="keyword">= </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">open</span><span class="keyword">(</span><span class="string">&quot;test.zip&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; if (</span><span class="default">$zip_status </span><span class="keyword">=== </span><span class="default">true</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">setPassword</span><span class="keyword">(</span><span class="string">&quot;MySecretPassword&quot;</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">extractTo</span><span class="keyword">(</span><span class="default">__DIR__</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Extraction failed (wrong password?)&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; die(</span><span class="string">&quot;Failed opening archive: &quot;</span><span class="keyword">. @</span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">getStatusString</span><span class="keyword">() . </span><span class="string">&quot; (code: &quot;</span><span class="keyword">. </span><span class="default">$zip_status </span><span class="keyword">.</span><span class="string">&quot;)&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.setpassword.php)

**[⬆ to root](/)**