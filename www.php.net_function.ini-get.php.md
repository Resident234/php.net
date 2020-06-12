# ini_get




<div class="phpcode"><span class="html">
another version of return_bytes which returns faster and does not use multiple multiplications (sorry:). even if it is resolved at compile time it is not a good practice;<br>no local variables are allocated;<br>the trim() is omitted (php already trimmed values when reading php.ini file);<br>strtolower() is replaced by second case which wins us one more function call for the price of doubling the number of cases to process (may slower the worst-case scenario when ariving to default: takes six comparisons instead of three comparisons and a function call);<br>cases are ordered by most frequent goes first (uppercase M-values being the default sizes);<br>specs say we must handle integer sizes so float values are converted to integers and 0.8G becomes 0;<br>&apos;Gb&apos;, &apos;Mb&apos;, &apos;Kb&apos; shorthand byte options are not implemented since are not in specs, see<br><a href="http://www.php.net/manual/en/faq.using.php#faq.using.shorthandbytes" rel="nofollow" target="_blank">http://www.php.net/manual/en/faq.using.php#faq.using.shorthandbytes</a><br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">return_bytes </span><span class="keyword">(</span><span class="default">$size_str</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; switch (</span><span class="default">substr </span><span class="keyword">(</span><span class="default">$size_str</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;M&apos;</span><span class="keyword">: case </span><span class="string">&apos;m&apos;</span><span class="keyword">: return (int)</span><span class="default">$size_str </span><span class="keyword">* </span><span class="default">1048576</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;K&apos;</span><span class="keyword">: case </span><span class="string">&apos;k&apos;</span><span class="keyword">: return (int)</span><span class="default">$size_str </span><span class="keyword">* </span><span class="default">1024</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;G&apos;</span><span class="keyword">: case </span><span class="string">&apos;g&apos;</span><span class="keyword">: return (int)</span><span class="default">$size_str </span><span class="keyword">* </span><span class="default">1073741824</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; default: return </span><span class="default">$size_str</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ini-get.php)

**[To root](/)**