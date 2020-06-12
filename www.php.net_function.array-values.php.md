# array_values




<div class="phpcode"><span class="html">
Remember, array_values() will ignore your beautiful numeric indexes, it will renumber them according tho the &apos;foreach&apos; ordering:<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= array(<br> </span><span class="default">3 </span><span class="keyword">=&gt; </span><span class="default">11</span><span class="keyword">,<br> </span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="default">22</span><span class="keyword">,<br> </span><span class="default">2 </span><span class="keyword">=&gt; </span><span class="default">33</span><span class="keyword">,<br>);<br></span><span class="default">$a</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="default">44</span><span class="keyword">;<br><br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">array_values</span><span class="keyword">( </span><span class="default">$a </span><span class="keyword">));<br>==&gt;<br>Array(<br>&#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="default">11<br>&#xA0; </span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] =&gt; </span><span class="default">22<br>&#xA0; </span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] =&gt; </span><span class="default">33<br>&#xA0; </span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] =&gt; </span><span class="default">44<br></span><span class="keyword">)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a warning that re-indexing an array by array_values() may cause you to reach the memory limit unexpectly.
<br>
<br>For example, if your PHP momory_limits is 8MB,
<br> and says there&apos;s a BIG array $bigArray which allocate 5MB of memory.
<br>
<br>Doing this will cause PHP exceeds the momory limits:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; $bigArray </span><span class="keyword">= </span><span class="default">array_values</span><span class="keyword">( </span><span class="default">$bigArray </span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>It&apos;s because array_values() does not re-index $bigArray directly,
<br>it just re-index it into another array, and assign to itself later.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is another way to get value from a multidimensional array, but for versions of php &gt;= 5.3.x<br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Get all values from specific key in a multidimensional array<br> *<br> * @param $key string<br> * @param $arr array<br> * @return null|string|array<br> */<br></span><span class="keyword">function </span><span class="default">array_value_recursive</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, array </span><span class="default">$arr</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$val </span><span class="keyword">= array();<br>&#xA0; &#xA0; </span><span class="default">array_walk_recursive</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, function(</span><span class="default">$v</span><span class="keyword">, </span><span class="default">$k</span><span class="keyword">) use(</span><span class="default">$key</span><span class="keyword">, &amp;</span><span class="default">$val</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$k </span><span class="keyword">== </span><span class="default">$key</span><span class="keyword">) </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">, </span><span class="default">$v</span><span class="keyword">);<br>&#xA0; &#xA0; });<br>&#xA0; &#xA0; return </span><span class="default">count</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) &gt; </span><span class="default">1 </span><span class="keyword">? </span><span class="default">$val </span><span class="keyword">: </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">);<br>}<br><br></span><span class="default">$arr </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;foo&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;foo&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;bar&apos; </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;baz&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;baz&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;candy&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;candy&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;vegetable&apos; </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;carrot&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;carrot&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; ),<br>&#xA0; &#xA0; </span><span class="string">&apos;vegetable&apos; </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;carrot&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;carrot2&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; ),<br>&#xA0; &#xA0; </span><span class="string">&apos;fruits&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;fruits&apos;</span><span class="keyword">,<br>);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_value_recursive</span><span class="keyword">(</span><span class="string">&apos;carrot&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">)); </span><span class="comment">// array(2) { [0]=&gt; string(6) &quot;carrot&quot; [1]=&gt; string(7) &quot;carrot2&quot; }<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_value_recursive</span><span class="keyword">(</span><span class="string">&apos;apple&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">)); </span><span class="comment">// null<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_value_recursive</span><span class="keyword">(</span><span class="string">&apos;baz&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">)); </span><span class="comment">// string(3) &quot;baz&quot;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_value_recursive</span><span class="keyword">(</span><span class="string">&apos;candy&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">)); </span><span class="comment">// string(5) &quot;candy&quot;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_value_recursive</span><span class="keyword">(</span><span class="string">&apos;pear&apos;</span><span class="keyword">, </span><span class="default">$arr</span><span class="keyword">)); </span><span class="comment">// null<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-values.php)

**[⬆ to root](/)**