# clearstatcache




<div class="phpcode"><span class="html">
unlink() does not clear the cache if you are performing file_exists() on a remote file like:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if (</span><span class="default">file_exists</span><span class="keyword">(</span><span class="string">&quot;<a href="ftp://ftp.example.com/somefile" rel="nofollow" target="_blank">ftp://ftp.example.com/somefile</a>&quot;</span><span class="keyword">))
<br></span><span class="default">?&gt;
<br></span>
<br>In this case, even after you unlink() successfully, you must call clearstatcache().
<br>
<br><span class="default">&lt;?php
<br>unlink</span><span class="keyword">(</span><span class="string">&quot;<a href="ftp://ftp.example.com/somefile" rel="nofollow" target="_blank">ftp://ftp.example.com/somefile</a>&quot;</span><span class="keyword">);
<br></span><span class="default">clearstatcache</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>file_exists() then properly returns false.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.clearstatcache.php)

**[To root](/README.md)**