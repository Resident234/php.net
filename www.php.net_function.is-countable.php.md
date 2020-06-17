# is_countable




<div class="phpcode"><span class="html">
If you are unable to upgrade to PHP 7.3 (not released at the time of writing), you can use this simple polyfill:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;is_countable&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; function </span><span class="default">is_countable</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) || </span><span class="default">$var </span><span class="keyword">instanceof </span><span class="default">Countable</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-countable.php)

**[To root](/README.md)**