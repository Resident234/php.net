# SplFileInfo::getBasename




<div class="phpcode"><span class="html">
If you want to get only filename and dont want to use weird:<br><br><span class="default">&lt;?php<br>pathinfo</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">getBasename</span><span class="keyword">(), </span><span class="default">PATHINFO_FILENAME</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>You can use (also weird but ~better looking):<br><br><span class="default">&lt;?php<br>$file</span><span class="keyword">-&gt;</span><span class="default">getBasename</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">.</span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">getExtension</span><span class="keyword">());<br></span><span class="default">?&gt;<br></span><br>PS: Why there is getFilename ? when it returns ~same stuff as getBasename ? I have to do this ugly stuff^ instead of simple getFilename...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/splfileinfo.getbasename.php)

**[To root](/README.md)**