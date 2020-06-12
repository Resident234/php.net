# BCMath Arbitrary Precision Mathematics




<div class="phpcode"><span class="html">
This extension is an interface to the GNU implementation as a library of the Basic Calculator utility by Philip Nelson; hence the name.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that when you use implementation of factorial that ClaudiuS made, you get results even if you try to calculate factorial of number that you normally can&apos;t, e.g. 2.5, -2, etc. Here is safer implementation:<br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Calculates a factorial of given number.<br> * @param string|int $num<br> * @throws InvalidArgumentException<br> * @return string<br> */<br></span><span class="keyword">function </span><span class="default">bcfact</span><span class="keyword">(</span><span class="default">$num</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if (!</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$num</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_INT</span><span class="keyword">) || </span><span class="default">$num </span><span class="keyword">&lt;= </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">InvalidArgumentException</span><span class="keyword">(</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;Argument must be natural number, &quot;%s&quot; given.&apos;</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">));<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; for (</span><span class="default">$result </span><span class="keyword">= </span><span class="string">&apos;1&apos;</span><span class="keyword">; </span><span class="default">$num </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">; </span><span class="default">$num</span><span class="keyword">--) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">bcmul</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Needed to compute some permutations and found the BC extension great but poor on functions, so untill this gets implemented here&apos;s the factorial function:<br><br><span class="default">&lt;?php<br></span><span class="comment">/* BC FACTORIAL<br> * n! = n * (n-1) * (n-2) .. 1 [eg. 5! = 5 * 4 * 3 * 2 * 1 = 120]<br> */<br></span><span class="keyword">function </span><span class="default">bcfact</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$factorial</span><span class="keyword">=</span><span class="default">$n</span><span class="keyword">;<br>&#xA0; &#xA0; while (--</span><span class="default">$n</span><span class="keyword">&gt;</span><span class="default">1</span><span class="keyword">) </span><span class="default">$factorial</span><span class="keyword">=</span><span class="default">bcmul</span><span class="keyword">(</span><span class="default">$factorial</span><span class="keyword">,</span><span class="default">$n</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$factorial</span><span class="keyword">;<br>}<br><br>print </span><span class="default">bcfact</span><span class="keyword">(</span><span class="default">50</span><span class="keyword">); <br></span><span class="comment">//30414093201713378043612608166064768844377641568960512000000000000<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.bc.php)

**[⬆ to root](/)**