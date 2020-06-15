# mkdir




<div class="phpcode"><span class="html">
When using the recursive parameter bear in mind that if you&apos;re using chmod() after mkdir() to set the mode without it being modified by the value of uchar() you need to call chmod() on all created directories. ie:<br><br><span class="default">&lt;?php<br>mkdir</span><span class="keyword">(</span><span class="string">&apos;/test1/test2&apos;</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">chmod</span><span class="keyword">(</span><span class="string">&apos;/test1/test2&apos;</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">);<br></span><span class="default">?&gt;</span> <br><br>May result in &quot;/test1/test2&quot; having a mode of 0777 but &quot;/test1&quot; still having a mode of 0755 from the mkdir() call. You&apos;d need to do:<br><br><span class="default">&lt;?php<br>mkdir</span><span class="keyword">(</span><span class="string">&apos;/test1/test2&apos;</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">chmod</span><span class="keyword">(</span><span class="string">&apos;/test1&apos;</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">);<br></span><span class="default">chmod</span><span class="keyword">(</span><span class="string">&apos;/test1/test2&apos;</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is an annotation from Stig Bakken:
<br>
<br>The mode on your directory is affected by your current umask.&#xA0; It will end
<br>up having (&lt;mkdir-mode&gt; and (not &lt;umask&gt;)).&#xA0; If you want to create one
<br>that is publicly readable, do something like this:
<br>
<br><span class="default">&lt;?php
<br>$oldumask </span><span class="keyword">= </span><span class="default">umask</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br></span><span class="default">mkdir</span><span class="keyword">(</span><span class="string">&apos;mydir&apos;</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">); </span><span class="comment">// or even 01777 so you get the sticky bit set
<br></span><span class="default">umask</span><span class="keyword">(</span><span class="default">$oldumask</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mkdir.php)

**[To root](/README.md)**