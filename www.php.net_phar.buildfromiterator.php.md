# Phar::buildFromIterator




<div class="phpcode"><span class="html">
You have to set a flag on the RecursiveDirectoryIterator because by default, the current (&quot;.&quot;) and parent directory (&quot;..&quot;) are included in the listing. This leads to an error message similar to &quot;returned a path &quot;..&quot; that is not in the base directory&quot;.<br><br>To fix this, use &quot;SKIP_DOTS&quot;:<br><br><span class="default">&lt;?php<br></span><span class="keyword">new </span><span class="default">RecursiveDirectoryIterator</span><span class="keyword">(<br>&#xA0; &#xA0; </span><span class="default">$srcRoot</span><span class="keyword">, </span><span class="default">FilesystemIterator</span><span class="keyword">::</span><span class="default">SKIP_DOTS<br></span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/phar.buildfromiterator.php)

**[⬆ to root](/)**