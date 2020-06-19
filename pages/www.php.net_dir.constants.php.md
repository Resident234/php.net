# Predefined Constants




<div class="phpcode"><span class="html">
In PHP 5.6 you can make a variadic function.<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Builds a file path with the appropriate directory separator.<br> * @param string $segments,... unlimited number of path segments<br> * @return string Path<br> */<br></span><span class="keyword">function </span><span class="default">file_build_path</span><span class="keyword">(...</span><span class="default">$segments</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="default">join</span><span class="keyword">(</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">, </span><span class="default">$segments</span><span class="keyword">);<br>}<br><br></span><span class="default">file_build_path</span><span class="keyword">(</span><span class="string">&quot;home&quot;</span><span class="keyword">, </span><span class="string">&quot;alice&quot;</span><span class="keyword">, </span><span class="string">&quot;Documents&quot;</span><span class="keyword">, </span><span class="string">&quot;example.txt&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>In earlier PHP versions you can use func_get_args.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">file_build_path</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="default">join</span><span class="keyword">(</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">, </span><span class="default">func_get_args</span><span class="keyword">(</span><span class="default">$segments</span><span class="keyword">));<br>}<br><br></span><span class="default">file_build_path</span><span class="keyword">(</span><span class="string">&quot;home&quot;</span><span class="keyword">, </span><span class="string">&quot;alice&quot;</span><span class="keyword">, </span><span class="string">&quot;Documents&quot;</span><span class="keyword">, </span><span class="string">&quot;example.txt&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For my part I&apos;ll continue to use this constant because it seems more future safe and flexible, even if Windows installations currently convert the paths magically. Not that syntax aesthetics matter but I think it can be made to look attractive:<br><br><span class="default">&lt;?php<br>$path </span><span class="keyword">= </span><span class="default">join</span><span class="keyword">(</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">, array(</span><span class="string">&apos;root&apos;</span><span class="keyword">, </span><span class="string">&apos;lib&apos;</span><span class="keyword">, </span><span class="string">&apos;file.php&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/dir.constants.php)

**[To root](/README.md)**