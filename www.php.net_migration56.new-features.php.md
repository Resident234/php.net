# New features




<div class="phpcode"><span class="html">
It is also possible ( in 5.6.0alpha ) to typehint the ...-operator<br><br>function foo (stdclass ... $inbound) {<br>&#xA0;&#xA0; var_dump($inbound);<br>}<br><br>// ok:<br>foo( (object)[&apos;foo&apos; =&gt; &apos;bar&apos;], (object)[&apos;bar&apos; =&gt; &apos;foo&apos;] );<br><br>// fails:<br>foo( 1, 2, 3, 4 );</span>
</div>
  

#


<div class="phpcode"><span class="html">
Remember, that<br><br>&#xA0; &#xA0; ($a ** $b) ** $c === $a ** ($b * $c)<br><br>Thats why exponent operator** is RIGHT associative.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note the order of operations in that exponentiation operator, as it was opposite of what my first expectation was:<br><br><span class="default">&lt;?php<br></span><span class="comment">// what I had expected, <br>// evaluating left to right, <br>// since no parens were used to guide the order of operations<br></span><span class="default">2 </span><span class="keyword">** </span><span class="default">3 </span><span class="keyword">** </span><span class="default">2 </span><span class="keyword">== </span><span class="default">64</span><span class="keyword">; </span><span class="comment">// (2 ** 3) ** 2 = 8 ** 2 = 64<br><br>// the given example, <br>// which appears to evaluate right to left<br></span><span class="default">2 </span><span class="keyword">** </span><span class="default">3 </span><span class="keyword">** </span><span class="default">2 </span><span class="keyword">== </span><span class="default">512</span><span class="keyword">; </span><span class="comment">// 2 ** (3 ** 2) = 2 ** 9 = 512<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">array_zip</span><span class="keyword">(...</span><span class="default">$arrays</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="default">array_merge</span><span class="keyword">(...</span><span class="default">array_map</span><span class="keyword">(</span><span class="default">NULL</span><span class="keyword">, ...</span><span class="default">$arrays</span><span class="keyword">));<br>}<br><br></span><span class="default">$a </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, </span><span class="default">7</span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= array(</span><span class="default">2</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">, </span><span class="default">8</span><span class="keyword">);<br></span><span class="default">$c </span><span class="keyword">= array(</span><span class="default">3</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">, </span><span class="default">9</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;, &apos;</span><span class="keyword">, </span><span class="default">array_zip</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">, </span><span class="default">$c</span><span class="keyword">)));<br><br></span><span class="comment">// Output<br></span><span class="default">string</span><span class="keyword">(</span><span class="default">25</span><span class="keyword">) </span><span class="string">&quot;1, 2, 3, 4, 5, 6, 7, 8, 9&quot;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration56.new-features.php)

**[⬆ to root](/)**