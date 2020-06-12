# array_shift




<div class="phpcode"><span class="html">
Using array_shift over larger array was fairly slow.&#xA0; It sped up as the array shrank, most likely as it has to reindex a smaller data set.<br><br>For my purpose, I used array_reverse, then array_pop, which doesn&apos;t need to reindex the array and will preserve keys if you want it to (didn&apos;t matter in my case).&#xA0; <br><br>Using direct index references, i.e., array_test[$i], was fast, but direct index referencing + unset for destructive operations was about the same speed as array_reverse and array_pop.&#xA0; It also requires sequential numeric keys.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Notice:<br>the complexity of array_pop() is O(1). <br>the complexity of array_shift() is O(n).<br>array_shift() requires a re-index process on the array, so it has to run over all the elements and index them.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a useful version which returns a simple array with the first key and value. Porbably a better way of doing it, but it works for me ;-)<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">array_kshift</span><span class="keyword">(&amp;</span><span class="default">$arr</span><span class="keyword">)<br>{<br>&#xA0; list(</span><span class="default">$k</span><span class="keyword">) = </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br>&#xA0; </span><span class="default">$r&#xA0; </span><span class="keyword">= array(</span><span class="default">$k</span><span class="keyword">=&gt;</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$k</span><span class="keyword">]);<br>&#xA0; unset(</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$k</span><span class="keyword">]);<br>&#xA0; return </span><span class="default">$r</span><span class="keyword">;<br>}<br><br></span><span class="comment">// test it on a simple associative array<br></span><span class="default">$arr </span><span class="keyword">= array(</span><span class="string">&apos;x&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;ball&apos;</span><span class="keyword">,</span><span class="string">&apos;y&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;hat&apos;</span><span class="keyword">,</span><span class="string">&apos;z&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;apple&apos;</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_kshift</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">));<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Output:<br><br>Array<br>(<br>&#xA0; &#xA0; [x] =&gt; ball<br>&#xA0; &#xA0; [y] =&gt; hat<br>&#xA0; &#xA0; [z] =&gt; apple<br>)<br>Array<br>(<br>&#xA0; &#xA0; [x] =&gt; ball<br>)<br>Array<br>(<br>&#xA0; &#xA0; [y] =&gt; hat<br>&#xA0; &#xA0; [z] =&gt; apple<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-shift.php)

**[⬆ to root](/)**