# basename




<div class="phpcode"><span class="html">
Support of the $suffix parameter has changed between PHP4 and PHP5:<br>in PHP4, $suffix is removed first, and then the core basename is applied.<br>conversely, in PHP5, $suffix is removed AFTER applying core basename.<br><br>Example:<br><span class="default">&lt;?php<br>&#xA0; $file </span><span class="keyword">= </span><span class="string">&quot;path/to/file.xml#xpointer(/Texture)&quot;</span><span class="keyword">;<br>&#xA0; echo </span><span class="default">basename</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="string">&quot;.xml#xpointer(/Texture)&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Result in PHP4: file<br>Result in PHP5: Texture)</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s a shame, that for a 20 years of development we don&apos;t have mb_basename() yet!<br><br>// works both in windows and unix<br>function mb_basename($path) {<br>&#xA0; &#xA0; if (preg_match(&apos;@^.*[\\\\/]([^\\\\/]+)$@s&apos;, $path, $matches)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return $matches[1];<br>&#xA0; &#xA0; } else if (preg_match(&apos;@^([^\\\\/]+)$@s&apos;, $path, $matches)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return $matches[1];<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return &apos;&apos;;<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is only one variant that works in my case for my Russian UTF-8 letters:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mb_basename</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; return </span><span class="default">end</span><span class="keyword">(</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;/&apos;</span><span class="keyword">,</span><span class="default">$file</span><span class="keyword">));
<br>}
<br>&gt;&lt;
<br>
<br></span><span class="default">It is intented </span><span class="keyword">for </span><span class="default">UNIX servers</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.basename.php)

**[⬆ to root](/)**