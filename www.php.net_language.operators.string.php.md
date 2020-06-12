# String Operators




<div class="phpcode"><span class="html">
As for me, curly braces serve good substitution for concatenation, and they are quicker to type and code looks cleaner. Remember to use double quotes (&quot; &quot;) as their content is parced by php, because in single quotes (&apos; &apos;) you&apos;ll get litaral name of variable provided:<br><br><span class="default">&lt;?php<br><br> $a </span><span class="keyword">= </span><span class="string">&apos;12345&apos;</span><span class="keyword">;<br><br></span><span class="comment">// This works:<br> </span><span class="keyword">echo </span><span class="string">&quot;qwe</span><span class="keyword">{</span><span class="default">$a</span><span class="keyword">}</span><span class="string">rty&quot;</span><span class="keyword">; </span><span class="comment">// qwe12345rty, using braces<br> </span><span class="keyword">echo </span><span class="string">&quot;qwe&quot; </span><span class="keyword">. </span><span class="default">$a </span><span class="keyword">. </span><span class="string">&quot;rty&quot;</span><span class="keyword">; </span><span class="comment">// qwe12345rty, concatenation used<br><br>// Does not work:<br> </span><span class="keyword">echo </span><span class="string">&apos;qwe{$a}rty&apos;</span><span class="keyword">; </span><span class="comment">// qwe{$a}rty, single quotes are not parsed<br> </span><span class="keyword">echo </span><span class="string">&quot;qwe</span><span class="default">$arty</span><span class="string">&quot;</span><span class="keyword">; </span><span class="comment">// qwe, because $a became $arty, which is undefined<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A word of caution - the dot operator has the same precedence as + and -, which can yield unexpected results. <br><br>Example:<br><br>&lt;php<br>$var = 3;<br><br>echo &quot;Result: &quot; . $var + 3;<br>?&gt;<br><br>The above will print out &quot;3&quot; instead of &quot;Result: 6&quot;, since first the string &quot;Result3&quot; is created and this is then added to 3 yielding 3, non-empty non-numeric strings being converted to 0.<br><br>To print &quot;Result: 6&quot;, use parantheses to alter precedence:<br><br>&lt;php<br>$var = 3;<br><br>echo &quot;Result: &quot; . ($var + 3); <br>?&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php <br></span><span class="string">&quot;</span><span class="keyword">{</span><span class="default">$str1</span><span class="keyword">}{</span><span class="default">$str2</span><span class="keyword">}{</span><span class="default">$str3</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">; </span><span class="comment">// one concat = fast<br>&#xA0; </span><span class="default">$str1</span><span class="keyword">. </span><span class="default">$str2</span><span class="keyword">. </span><span class="default">$str3</span><span class="keyword">;&#xA0;&#xA0; </span><span class="comment">// two concats = slow<br></span><span class="default">?&gt;<br></span>Use double quotes to concat more than two strings instead of multiple &apos;.&apos; operators.&#xA0; PHP is forced to re-concatenate with every &apos;.&apos; operator.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you attempt to add numbers with a concatenation operator, your result will be the result of those numbers as strings.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">echo </span><span class="string">&quot;thr&quot;</span><span class="keyword">.</span><span class="string">&quot;ee&quot;</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">//prints the string &quot;three&quot;<br></span><span class="keyword">echo </span><span class="string">&quot;twe&quot; </span><span class="keyword">. </span><span class="string">&quot;lve&quot;</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//prints the string &quot;twelve&quot;<br></span><span class="keyword">echo </span><span class="default">1 </span><span class="keyword">. </span><span class="default">2</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//prints the string &quot;12&quot;<br></span><span class="keyword">echo </span><span class="default">1.2</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//prints the number 1.2<br></span><span class="keyword">echo </span><span class="default">1</span><span class="keyword">+</span><span class="default">2</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//prints the number 3<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful so that you don&apos;t type &quot;.&quot; instead of &quot;;&quot; at the end of a line.<br><br>It took me more than 30 minutes to debug a long script because of something like this:<br><br>&lt;?<br>echo &apos;a&apos;.<br>$c = &apos;x&apos;;<br>echo &apos;b&apos;;<br>echo &apos;c&apos;;<br>?&gt;<br><br>The output is &quot;axbc&quot;, because of the dot on the first line.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.string.php)

**[To root](/)**