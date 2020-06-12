# round




<div class="phpcode"><span class="html">
In my opinion this function lacks two flags:<br><br>- PHP_ROUND_UP - Always round up.<br>- PHP_ROUND_DOWN - Always round down.<br><br>In accounting, it&apos;s often necessary to always round up, or down to a precision of thousandths.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">round_up</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="default">$precision </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$fig </span><span class="keyword">= (int) </span><span class="default">str_pad</span><span class="keyword">(</span><span class="string">&apos;1&apos;</span><span class="keyword">, </span><span class="default">$precision</span><span class="keyword">, </span><span class="string">&apos;0&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; return (</span><span class="default">ceil</span><span class="keyword">(</span><span class="default">$number </span><span class="keyword">* </span><span class="default">$fig</span><span class="keyword">) / </span><span class="default">$fig</span><span class="keyword">);<br>}<br><br>function </span><span class="default">round_down</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="default">$precision </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$fig </span><span class="keyword">= (int) </span><span class="default">str_pad</span><span class="keyword">(</span><span class="string">&apos;1&apos;</span><span class="keyword">, </span><span class="default">$precision</span><span class="keyword">, </span><span class="string">&apos;0&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; return (</span><span class="default">floor</span><span class="keyword">(</span><span class="default">$number </span><span class="keyword">* </span><span class="default">$fig</span><span class="keyword">) / </span><span class="default">$fig</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As PHP doesn&apos;t have a a native number truncate function, this is my solution - a function that can be usefull if you need truncate instead round a number.<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Truncate a float number, example: &lt;code&gt;truncate(-1.49999, 2); // returns -1.49<br> * truncate(.49999, 3); // returns 0.499<br> * &lt;/code&gt;<br> * @param float $val Float number to be truncate<br> * @param int f Number of precision<br> * @return float<br> */<br></span><span class="keyword">function </span><span class="default">truncate</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">, </span><span class="default">$f</span><span class="keyword">=</span><span class="string">&quot;0&quot;</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if((</span><span class="default">$p </span><span class="keyword">= </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">, </span><span class="string">&apos;.&apos;</span><span class="keyword">)) !== </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$val </span><span class="keyword">= </span><span class="default">floatval</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$p </span><span class="keyword">+ </span><span class="default">1 </span><span class="keyword">+ </span><span class="default">$f</span><span class="keyword">));<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$val</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Originally posted in <a href="http://stackoverflow.com/a/12710283/1596489" rel="nofollow" target="_blank">http://stackoverflow.com/a/12710283/1596489</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
If you have negative zero and you need return positive number simple add +0:<br><br>$number = -2.38419e-07;<br>var_dump(round($number,1));//float(-0)<br>var_dump(round($number,1) + 0);//float(0)</span>
</div>
  

#


<div class="phpcode"><span class="html">
I discovered that under some conditions you can get rounding errors with round when converting the number to a string afterwards.<br><br>To fix this I swapped round() for number_format().<br><br>Unfortunately i cant give an example (because the number cant be represented as a string !)<br><br>essentially I had round(0.688888889,2);<br><br>which would stay as 0.68888889 when printed as a string.<br><br>But using number_format it correctly became 0.69.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;d only want to round for displaying variables (not for calculating on the rounded result) then you should use printf with the float:
<br>
<br><span class="default">&lt;?php printf </span><span class="keyword">(</span><span class="string">&quot;%6.2f&quot;</span><span class="keyword">,</span><span class="default">3.39532</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>This returns: 3.40 .</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is function that rounds to a specified increment, but always up. I had to use it for price adjustment that always went up to $5 increments.
<br>
<br><span class="default">&lt;?php&#xA0; 
<br></span><span class="keyword">function </span><span class="default">roundUpTo</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="default">$increments</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$increments </span><span class="keyword">= </span><span class="default">1 </span><span class="keyword">/ </span><span class="default">$increments</span><span class="keyword">;
<br>&#xA0; &#xA0; return (</span><span class="default">ceil</span><span class="keyword">(</span><span class="default">$number </span><span class="keyword">* </span><span class="default">$increments</span><span class="keyword">) / </span><span class="default">$increments</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.round.php)

**[⬆ to root](/)**