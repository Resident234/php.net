# apc_store




<div class="phpcode"><span class="html">
if you want to store array of objects in apc use ArrayObject wrapper (PHP5).<br><br><span class="default">&lt;?php<br>$objs </span><span class="keyword">= array();<br></span><span class="default">$objs</span><span class="keyword">[] = new </span><span class="default">TestClass</span><span class="keyword">();<br></span><span class="default">$objs</span><span class="keyword">[] = new </span><span class="default">TestClass</span><span class="keyword">();<br></span><span class="default">$objs</span><span class="keyword">[] = new </span><span class="default">TestClass</span><span class="keyword">();<br><br></span><span class="comment">//Doesn&apos;t work<br></span><span class="default">apc_store</span><span class="keyword">(</span><span class="string">&apos;objs&apos;</span><span class="keyword">,</span><span class="default">$objs</span><span class="keyword">,</span><span class="default">60</span><span class="keyword">);<br></span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">apc_fetch</span><span class="keyword">(</span><span class="string">&apos;objs&apos;</span><span class="keyword">); <br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$tmp</span><span class="keyword">);<br><br></span><span class="comment">//Works<br></span><span class="default">apc_store</span><span class="keyword">(</span><span class="string">&apos;objs&apos;</span><span class="keyword">,new </span><span class="default">ArrayObject</span><span class="keyword">(</span><span class="default">$objs</span><span class="keyword">),</span><span class="default">60</span><span class="keyword">);<br></span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">apc_fetch</span><span class="keyword">(</span><span class="string">&apos;objs&apos;</span><span class="keyword">); <br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$tmp</span><span class="keyword">-&gt;</span><span class="default">getArrayCopy</span><span class="keyword">());<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.apc-store.php)

**[To root](/)**