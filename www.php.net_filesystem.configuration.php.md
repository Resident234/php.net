# Runtime Configuration




<div class="phpcode"><span class="html">
I&apos;m surprised this isn&apos;t mentioned in docs here, but to set these values at runtime use &quot;ini_set()&quot;. For example:<br><br><span class="default">&lt;?php<br>ini_set</span><span class="keyword">(</span><span class="string">&quot;auto_detect_line_endings&quot;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="comment">// Now I can invoke fgets() on files that contain silly \r line endings. <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/filesystem.configuration.php)

**[⬆ to root](/)**