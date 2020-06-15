# ftp_fput




<div class="phpcode"><span class="html">
For directly inserting content into a file on an FTP host, you could also create a string stream wich streams directly to the ftp_fput function. <br><br>This should create less overhead than first writing to any temp directories locally before streaming, as suggested here.<br><br><span class="default">&lt;?php<br><br>$string </span><span class="keyword">= </span><span class="string">&quot;Your content goes here&quot;</span><span class="keyword">;<br></span><span class="default">$stream </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;data://text/plain,&apos; </span><span class="keyword">. </span><span class="default">$string</span><span class="keyword">,</span><span class="string">&apos;r&apos;</span><span class="keyword">);<br><br></span><span class="default">ftp_fput</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">connection</span><span class="keyword">,</span><span class="default">$pathTo</span><span class="keyword">,</span><span class="default">$stream</span><span class="keyword">, </span><span class="default">FTP_BINARY</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-fput.php)

**[To root](/README.md)**