# array_reverse




<div class="phpcode"><span class="html">
Needed to just reverse keys. Don&apos;t flog me if there is a better way. This was a simple solution.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">array_reverse_keys</span><span class="keyword">(</span><span class="default">$ar</span><span class="keyword">){
<br>&#xA0; &#xA0; return </span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">$ar</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">),</span><span class="default">false</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-reverse.php)

**[To root](/README.md)**