# disk_free_space




<div class="phpcode"><span class="html">
Transformation is possible WITHOUT using loops:<br><br><span class="default">&lt;?php <br>&#xA0; &#xA0; $bytes </span><span class="keyword">= </span><span class="default">disk_free_space</span><span class="keyword">(</span><span class="string">&quot;.&quot;</span><span class="keyword">); <br>&#xA0; &#xA0; </span><span class="default">$si_prefix </span><span class="keyword">= array( </span><span class="string">&apos;B&apos;</span><span class="keyword">, </span><span class="string">&apos;KB&apos;</span><span class="keyword">, </span><span class="string">&apos;MB&apos;</span><span class="keyword">, </span><span class="string">&apos;GB&apos;</span><span class="keyword">, </span><span class="string">&apos;TB&apos;</span><span class="keyword">, </span><span class="string">&apos;EB&apos;</span><span class="keyword">, </span><span class="string">&apos;ZB&apos;</span><span class="keyword">, </span><span class="string">&apos;YB&apos; </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$base </span><span class="keyword">= </span><span class="default">1024</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$class </span><span class="keyword">= </span><span class="default">min</span><span class="keyword">((int)</span><span class="default">log</span><span class="keyword">(</span><span class="default">$bytes </span><span class="keyword">, </span><span class="default">$base</span><span class="keyword">) , </span><span class="default">count</span><span class="keyword">(</span><span class="default">$si_prefix</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">$bytes </span><span class="keyword">. </span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; echo </span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;%1.2f&apos; </span><span class="keyword">, </span><span class="default">$bytes </span><span class="keyword">/ </span><span class="default">pow</span><span class="keyword">(</span><span class="default">$base</span><span class="keyword">,</span><span class="default">$class</span><span class="keyword">)) . </span><span class="string">&apos; &apos; </span><span class="keyword">. </span><span class="default">$si_prefix</span><span class="keyword">[</span><span class="default">$class</span><span class="keyword">] . </span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Nice, but please be aware of the prefixes.<br><br>SI specifies a lower case &apos;k&apos; as 1&apos;000 prefix.<br>It doesn&apos;t make sense to use an upper case &apos;K&apos; as binary prefix,<br>while the decimal Mega (M and following) prefixes in SI are uppercase.<br>Furthermore, there are REAL binary prefixes since a few years.<br><br>Do it the (newest and recommended) &quot;IEC&quot; way:<br><br>KB&apos;s are calculated decimal; power of 10 (1000 bytes each)<br>KiB&apos;s are calculated binary; power of 2 (1024 bytes each).<br>The same goes for MB, MiB and so on...<br><br>Feel free to read:<br><a href="http://en.wikipedia.org/wiki/Binary_prefix" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Binary_prefix</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
$si_prefix = array( &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; );<br><br>you are missing the petabyte after terabyte<br><br> &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos; <br><br>should look like<br><br> &apos;B&apos;, &apos;KB&apos;, &apos;MB&apos;, &apos;GB&apos;, &apos;TB&apos;, &apos;PB&apos;, &apos;EB&apos;, &apos;ZB&apos;, &apos;YB&apos;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.disk-free-space.php)

**[To root](/README.md)**