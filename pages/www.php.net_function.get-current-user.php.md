# get_current_user




<div class="phpcode"><span class="html">
to get the username of the process owner (rather than the file owner), you can use:<br><br><span class="default">&lt;?php<br>$processUser </span><span class="keyword">= </span><span class="default">posix_getpwuid</span><span class="keyword">(</span><span class="default">posix_geteuid</span><span class="keyword">());<br>print </span><span class="default">$processUser</span><span class="keyword">[</span><span class="string">&apos;name&apos;</span><span class="keyword">];<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-current-user.php)

**[To root](/README.md)**