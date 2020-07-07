# crc32




<div class="phpcode"><span class="html">
This function returns an unsigned integer from a 64-bit Linux platform.&#xA0; It does return the signed integer from other 32-bit platforms even a 64-bit Windows one.
<br>
<br>The reason is because the two constants PHP_INT_SIZE and PHP_INT_MAX have different values on the 64-bit Linux platform.
<br>
<br>I&apos;ve created a work-around function to handle this situation.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">get_signed_int</span><span class="keyword">(</span><span class="default">$in</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$int_max </span><span class="keyword">= </span><span class="default">pow</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">, </span><span class="default">31</span><span class="keyword">)-</span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; if (</span><span class="default">$in </span><span class="keyword">&gt; </span><span class="default">$int_max</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$out </span><span class="keyword">= </span><span class="default">$in </span><span class="keyword">- </span><span class="default">$int_max </span><span class="keyword">* </span><span class="default">2 </span><span class="keyword">- </span><span class="default">2</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$out </span><span class="keyword">= </span><span class="default">$in</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$out</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.crc32.php)

**[To root](/README.md)**