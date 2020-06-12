# substr_compare




<div class="phpcode"><span class="html">
When you came to this page, you may have been looking for something a little simpler: A function that can check if a small string exists within a larger string starting at a particular index. Using substr_compare() for this can leave your code messy, because you need to check that your string is long enough (to avoid the warning), manually specify the length of the short string, and like many of the string functions, perform an integer comparison to answer a true/false question.<br><br>I put together a simple function to return true if $str exists within $mainStr. If $loc is specified, the $str must begin at that index. If not, the entire $mainStr will be searched.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">contains_substr</span><span class="keyword">(</span><span class="default">$mainStr</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">$loc </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">$loc </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">) return (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$mainStr</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">) !== </span><span class="default">false</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$mainStr</span><span class="keyword">) &lt; </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">)) return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; if ((</span><span class="default">$loc </span><span class="keyword">+ </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">)) &gt; </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$mainStr</span><span class="keyword">)) return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; return (</span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$mainStr</span><span class="keyword">, </span><span class="default">$loc</span><span class="keyword">, </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">)), </span><span class="default">$str</span><span class="keyword">) == </span><span class="default">0</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Take note of the `length` parameter: &quot;The default value is the largest of the length of the str compared to the length of main_str less the offset.&quot;<br><br>This is *not* the length of str as you might (I always) expect, so if you leave it out, you&apos;ll get unexpected results.&#xA0; Example:<br><br><span class="default">&lt;?php<br>$hash </span><span class="keyword">= </span><span class="string">&apos;$5$lalalalalalalala$crypt.output.here&apos;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">substr_compare</span><span class="keyword">(</span><span class="default">$hash</span><span class="keyword">, </span><span class="string">&apos;$5$&apos;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">)); </span><span class="comment"># int(34)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">substr_compare</span><span class="keyword">(</span><span class="default">$hash</span><span class="keyword">, </span><span class="string">&apos;$5$&apos;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">)); </span><span class="comment"># int(0)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">PHP_VERSION</span><span class="keyword">); </span><span class="comment"># string(6) &quot;5.3.14&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.substr-compare.php)

**[⬆ to root](/)**