# reset




<div class="phpcode"><span class="html">
GOTCHA: If your first element is false, you don&apos;t know whether it was empty or not.<br><br><span class="default">&lt;?php<br><br>$a </span><span class="keyword">= array();<br></span><span class="default">$b </span><span class="keyword">= array(</span><span class="default">false</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">reset</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">) === </span><span class="default">reset</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">)); </span><span class="comment">//bool(true)<br><br></span><span class="default">?&gt;<br></span><br>So don&apos;t count on a false return being an empty array.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.reset.php)

**[To root](/README.md)**