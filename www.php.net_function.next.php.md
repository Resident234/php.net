# next




<div class="phpcode"><span class="html">
Now from PHP 7.2, the function &quot;each&quot; is deprecated, so the has_next I&apos;ve posted is no longer a good idea. There is another to keep it simple and fast:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">has_next</span><span class="keyword">(array </span><span class="default">$_array</span><span class="keyword">)<br>{<br>&#xA0; return </span><span class="default">next</span><span class="keyword">(</span><span class="default">$_array</span><span class="keyword">) !== </span><span class="default">false </span><span class="keyword">?: </span><span class="default">key</span><span class="keyword">(</span><span class="default">$_array</span><span class="keyword">) !== </span><span class="default">null</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.next.php)

**[⬆ to root](/)**