# sprintf




<div class="phpcode"><span class="html">
1.&#xA0; A plus sign (&apos;+&apos;) means put a &apos;+&apos; before positive numbers while a minus sign (&apos;-&apos;) means left justify.&#xA0; The documentation incorrectly states that they are interchangeable.&#xA0; They produce unique results that can be combined:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%+4d|%+4d|\n&quot;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">1</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">);<br>echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%-4d|%-4d|\n&quot;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">1</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">);<br>echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%+-4d|%+-4d|\n&quot;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>outputs:<br><br>|&#xA0; +1|&#xA0; -1|<br>|1&#xA0;&#xA0; |-1&#xA0; |<br>|+1&#xA0; |-1&#xA0; |<br><br>2.&#xA0; Padding with a &apos;0&apos; is different than padding with other characters.&#xA0; Zeros will only be added at the front of a number, after any sign.&#xA0; Other characters will be added before the sign, or after the number:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%04d|\n&quot;</span><span class="keyword">,&#xA0;&#xA0; -</span><span class="default">2</span><span class="keyword">);<br>echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%&apos;:4d|\n&quot;</span><span class="keyword">,&#xA0; -</span><span class="default">2</span><span class="keyword">);<br>echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%-&apos;:4d|\n&quot;</span><span class="keyword">, -</span><span class="default">2</span><span class="keyword">);<br><br></span><span class="comment">// Specifying both &quot;-&quot; and &quot;0&quot; creates a conflict with unexpected results:<br></span><span class="keyword">echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%-04d|\n&quot;</span><span class="keyword">,&#xA0; -</span><span class="default">2</span><span class="keyword">);<br><br></span><span class="comment">// Padding with other digits behaves like other non-zero characters:<br></span><span class="keyword">echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%-&apos;14d|\n&quot;</span><span class="keyword">, -</span><span class="default">2</span><span class="keyword">);<br>echo </span><span class="default">sprintf </span><span class="keyword">(</span><span class="string">&quot;|%-&apos;04d|\n&quot;</span><span class="keyword">, -</span><span class="default">2</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>outputs:<br><br>|-002|<br>|::-2|<br>|-2::|<br>|-2&#xA0; |<br>|-211|<br>|-2&#xA0; |</span>
</div>
  

#


<div class="phpcode"><span class="html">
With printf() and sprintf() functions, escape character is not backslash &apos;\&apos; but rather &apos;%&apos;.<br><br>Ie. to print &apos;%&apos; character you need to escape it with itself:<br><span class="default">&lt;?php<br>printf</span><span class="keyword">(</span><span class="string">&apos;%%%s%%&apos;</span><span class="keyword">, </span><span class="string">&apos;koko&apos;</span><span class="keyword">); </span><span class="comment">#output: &apos;%koko%&apos;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There are already some comments on using sprintf to force leading leading zeros but the examples only include integers. I needed leading zeros on floating point numbers and was surprised that it didn&apos;t work as expected.<br><br>Example:<br><span class="default">&lt;?php<br>sprintf</span><span class="keyword">(</span><span class="string">&apos;%02d&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This will result in 01. However, trying the same for a float with precision doesn&apos;t work:<br><br><span class="default">&lt;?php<br>sprintf</span><span class="keyword">(</span><span class="string">&apos;%02.2f&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Yields 1.00. <br><br>This threw me a little off. To get the desired result, one needs to add the precision (2) and the length of the decimal seperator &quot;.&quot; (1). So the correct pattern would be<br><br><span class="default">&lt;?php<br>sprintf</span><span class="keyword">(</span><span class="string">&apos;%05.2f&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Output: 01.00<br><br>Please see <a href="http://stackoverflow.com/a/28739819/413531" rel="nofollow" target="_blank">http://stackoverflow.com/a/28739819/413531</a> for a more detailed explanation.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is how to print a floating point number with 16 significant digits regardless of magnitude:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $result </span><span class="keyword">= </span><span class="default">sprintf</span><span class="keyword">(</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;%%.%dF&apos;</span><span class="keyword">, </span><span class="default">max</span><span class="keyword">(</span><span class="default">15 </span><span class="keyword">- </span><span class="default">floor</span><span class="keyword">(</span><span class="default">log10</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">)), </span><span class="default">0</span><span class="keyword">)), </span><span class="default">$value</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This works more reliably than doing something like sprintf(&apos;%.15F&apos;, $value) as the latter may cut off significant digits for very small numbers, or prints bogus digits (meaning extra digits beyond what can reliably be represented in a floating point number) for very large numbers.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I created this function a while back to save on having to combine mysql_real_escape_string onto all the params passed into a sprintf. it works literally the same as the sprintf other than that it doesn&apos;t require you to escape your inputs. Hope its of some use to people<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">mressf</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; </span><span class="default">$args </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">();<br>&#xA0; &#xA0; if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">) &lt; </span><span class="default">2</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$query </span><span class="keyword">= </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$args </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;mysql_real_escape_string&apos;</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_unshift</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">, </span><span class="default">$query</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$query </span><span class="keyword">= </span><span class="default">call_user_func_array</span><span class="keyword">(</span><span class="string">&apos;sprintf&apos;</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$query</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Regards<br>Jay<br>Jaygilford.com</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.sprintf.php)

**[⬆ to root](/)**