# is_float




<div class="phpcode"><span class="html">
Coercing the value to float and back to string was a neat trick. You can also just add a literal 0 to whatever you&apos;re checking.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">isfloat</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; </span><span class="comment">// PHP automagically tries to coerce $value to a number<br>&#xA0; </span><span class="keyword">return </span><span class="default">is_float</span><span class="keyword">(</span><span class="default">$value </span><span class="keyword">+ </span><span class="default">0</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Seems to work ok:<br><br><span class="default">&lt;?php<br>isfloat</span><span class="keyword">(</span><span class="string">&quot;5.0&quot; </span><span class="keyword">+ </span><span class="default">0</span><span class="keyword">);&#xA0; </span><span class="comment">// true<br></span><span class="default">isfloat</span><span class="keyword">(</span><span class="string">&quot;5.0&quot;</span><span class="keyword">);&#xA0; </span><span class="comment">// false<br></span><span class="default">isfloat</span><span class="keyword">(</span><span class="default">5 </span><span class="keyword">+ </span><span class="default">0</span><span class="keyword">);&#xA0; </span><span class="comment">// false<br></span><span class="default">isfloat</span><span class="keyword">(</span><span class="default">5.0 </span><span class="keyword">+ </span><span class="default">0</span><span class="keyword">);&#xA0; </span><span class="comment">// false<br></span><span class="default">isfloat</span><span class="keyword">(</span><span class="string">&apos;a&apos; </span><span class="keyword">+ </span><span class="default">0</span><span class="keyword">);&#xA0; </span><span class="comment">// false<br></span><span class="default">?&gt;<br></span><br>YMMV</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to test whether a string is containing a float, rather than if a variable is a float, you can use this simple little function:<br><br>function isfloat($f) return ($f == (string)(float)$f);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-float.php)

**[To root](/README.md)**