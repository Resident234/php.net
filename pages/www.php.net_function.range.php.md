# range




<div class="phpcode"><span class="html">
To create a range array like<br><br>Array<br>(<br>&#xA0; &#xA0; [11] =&gt; 1<br>&#xA0; &#xA0; [12] =&gt; 2<br>&#xA0; &#xA0; [13] =&gt; 3<br>&#xA0; &#xA0; [14] =&gt; 4<br>)<br><br>combine two range arrays using array_combine:<br><br>array_combine(range(11,14),range(1,4))</span>
</div>
  

#


<div class="phpcode"><span class="html">
So with the introduction of single-character ranges to the range() function, the internal function tries to be &quot;smart&quot;, and (I am inferring from behavior here) apparently checks the type of the incoming values. If one is numeric, including numeric string, then the other is treated as numeric; if it is a non-numeric string, it is treated as zero. <br><br>But.<br><br>If you pass in a numeric string in such a way that is is forced to be recognized as type string and not type numeric, range() will function quite differently.<br><br>Compare:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">range</span><span class="keyword">(</span><span class="default">9</span><span class="keyword">,</span><span class="string">&quot;Q&quot;</span><span class="keyword">));<br></span><span class="comment">// prints 9876543210<br><br></span><span class="keyword">echo </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">range</span><span class="keyword">(</span><span class="string">&quot;9 &quot;</span><span class="keyword">,</span><span class="string">&quot;Q&quot;</span><span class="keyword">));&#xA0; </span><span class="comment">//space after the 9<br>// prints 9:;&lt;=&gt;?@ABCDEFGHIJKLMNOPQ<br><br></span><span class="keyword">echo </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">range</span><span class="keyword">(</span><span class="string">&quot;q&quot;</span><span class="keyword">,</span><span class="string">&quot;9 &quot;</span><span class="keyword">));<br></span><span class="comment">// prints qponmlkjihgfedcba`_^]\[ZYXWVUTSRQPONMLKJIHGFEDCBA@</span><span class="default">?&gt;</span>=&lt;;:987654<br>?&gt;<br><br>I wouldn&apos;t call this a bug, because IMO it is even more useful than the stock usage of the function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
foreach(range()) whilst efficiant in other languages, such as python, it is not (compared to a for) in php*.<br><br>php is a C-inspired language and thus for is entirely in-keeping with the lanuage aethetic to use it<br><br><span class="default">&lt;?php<br></span><span class="comment">//efficiant<br></span><span class="keyword">for(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">$start</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$end</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">+=</span><span class="default">$step</span><span class="keyword">) <br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//do something with array <br></span><span class="keyword">}<br><br></span><span class="comment">//inefficiant<br></span><span class="keyword">foreach(</span><span class="default">range</span><span class="keyword">(</span><span class="default">$start</span><span class="keyword">, </span><span class="default">$end</span><span class="keyword">, </span><span class="default">$step</span><span class="keyword">) as </span><span class="default">$i</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//do something with array <br></span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>That the officiant documentation doesnt mention the for loop is strange.<br><br>Note however, that in PHP5 foreach is faster than for when iterating without incrementing a variable.<br><br>* My tests using microtime and 100 000 iterations consistently (~10 times) show that for is 4x faster than foreach(range()).</span>
</div>
  

#


<div class="phpcode"><span class="html">
So, I needed a quick and dirty way to create a dropdown select for hours, minutes and seconds using 2 digit formatting, and to create those arrays of data, I combined range with array merge..
<br>
<br><span class="default">&lt;?php
<br>$prepend </span><span class="keyword">= array(</span><span class="string">&apos;00&apos;</span><span class="keyword">,</span><span class="string">&apos;01&apos;</span><span class="keyword">,</span><span class="string">&apos;02&apos;</span><span class="keyword">,</span><span class="string">&apos;03&apos;</span><span class="keyword">,</span><span class="string">&apos;04&apos;</span><span class="keyword">,</span><span class="string">&apos;05&apos;</span><span class="keyword">,</span><span class="string">&apos;06&apos;</span><span class="keyword">,</span><span class="string">&apos;07&apos;</span><span class="keyword">,</span><span class="string">&apos;08&apos;</span><span class="keyword">,</span><span class="string">&apos;09&apos;</span><span class="keyword">);
<br></span><span class="default">$hours&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$prepend</span><span class="keyword">,</span><span class="default">range</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">, </span><span class="default">23</span><span class="keyword">));
<br></span><span class="default">$minutes&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$prepend</span><span class="keyword">,</span><span class="default">range</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">, </span><span class="default">59</span><span class="keyword">));
<br></span><span class="default">$seconds&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">$minutes</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Super simple.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.range.php)

**[To root](/README.md)**