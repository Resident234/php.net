# similar_text




<div class="phpcode"><span class="html">
Hey there,
<br>
<br>Be aware when using this function, that the order of passing the strings is very important if you want to calculate the percentage of similarity, in fact, altering the variables will give a very different result, example :
<br>
<br><span class="default">&lt;?php
<br>$var_1 </span><span class="keyword">= </span><span class="string">&apos;PHP IS GREAT&apos;</span><span class="keyword">;
<br></span><span class="default">$var_2 </span><span class="keyword">= </span><span class="string">&apos;WITH MYSQL&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">similar_text</span><span class="keyword">(</span><span class="default">$var_1</span><span class="keyword">, </span><span class="default">$var_2</span><span class="keyword">, </span><span class="default">$percent</span><span class="keyword">);
<br>
<br>echo </span><span class="default">$percent</span><span class="keyword">;
<br></span><span class="comment">// 27.272727272727
<br>
<br></span><span class="default">similar_text</span><span class="keyword">(</span><span class="default">$var_2</span><span class="keyword">, </span><span class="default">$var_1</span><span class="keyword">, </span><span class="default">$percent</span><span class="keyword">);
<br>
<br>echo </span><span class="default">$percent</span><span class="keyword">;
<br></span><span class="comment">// 18.181818181818
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that this function calculates a similarity of 0 (zero) for two empty strings.<br><br><span class="default">&lt;?php<br>similar_text</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">, </span><span class="string">&quot;&quot;</span><span class="keyword">, </span><span class="default">$sim</span><span class="keyword">);<br>echo </span><span class="default">$sim</span><span class="keyword">; </span><span class="comment">// &quot;0&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Recursive algorithm usually is very elegant one. I found a way to get better precision without the recursion. Imagine two different (or same) length ribbons with letters on each. You simply shifting one ribbon to left till it matches the letter the first.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">similarity</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$len1 </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$len2 </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$max </span><span class="keyword">= </span><span class="default">max</span><span class="keyword">(</span><span class="default">$len1</span><span class="keyword">, </span><span class="default">$len2</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$similarity </span><span class="keyword">= </span><span class="default">$i </span><span class="keyword">= </span><span class="default">$j </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; while ((</span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$len1</span><span class="keyword">) &amp;&amp; isset(</span><span class="default">$str2</span><span class="keyword">[</span><span class="default">$j</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$str1</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] == </span><span class="default">$str2</span><span class="keyword">[</span><span class="default">$j</span><span class="keyword">]) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$similarity</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$j</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; } elseif (</span><span class="default">$len1 </span><span class="keyword">&lt; </span><span class="default">$len2</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$len1</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$j</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; } elseif (</span><span class="default">$len1 </span><span class="keyword">&gt; </span><span class="default">$len2</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$len1</span><span class="keyword">--;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$j</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return </span><span class="default">round</span><span class="keyword">(</span><span class="default">$similarity </span><span class="keyword">/ </span><span class="default">$max</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);<br>}<br><br></span><span class="default">$str1 </span><span class="keyword">= </span><span class="string">&apos;12345678901234567890&apos;</span><span class="keyword">;<br></span><span class="default">$str2 </span><span class="keyword">= </span><span class="string">&apos;12345678991234567890&apos;</span><span class="keyword">;<br><br>echo </span><span class="string">&apos;Similarity: &apos; </span><span class="keyword">. (</span><span class="default">similarity</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">) * </span><span class="default">100</span><span class="keyword">) . </span><span class="string">&apos;%&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that this function is case sensitive:<br><br><span class="default">&lt;?php<br><br>$var1 </span><span class="keyword">= </span><span class="string">&apos;Hello&apos;</span><span class="keyword">;<br></span><span class="default">$var2 </span><span class="keyword">= </span><span class="string">&apos;Hello&apos;</span><span class="keyword">;<br></span><span class="default">$var3 </span><span class="keyword">= </span><span class="string">&apos;hello&apos;</span><span class="keyword">;<br><br>echo </span><span class="default">similar_text</span><span class="keyword">(</span><span class="default">$var1</span><span class="keyword">, </span><span class="default">$var2</span><span class="keyword">);&#xA0; </span><span class="comment">// 5<br></span><span class="keyword">echo </span><span class="default">similar_text</span><span class="keyword">(</span><span class="default">$var1</span><span class="keyword">, </span><span class="default">$var3</span><span class="keyword">);&#xA0; </span><span class="comment">// 4</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Actually similar_text() is not bad...<br>it works good. But before processing i think is a good way to make a little mod like this<br><br>$var_1 = strtoupper(&quot;doggy&quot;);<br>$var_2 = strtoupper(&quot;Dog&quot;);<br><br>similar_text($var_1, $var_2, $percent); <br><br>echo $percent; // output is 75 but without strtoupper output is 50</span>
</div>
  

#


<div class="phpcode"><span class="html">
If performance is an issue, you may wish to use the levenshtein() function instead, which has a considerably better complexity of O(str1 * str2).</span>
</div>
  

#


<div class="phpcode"><span class="html">
The speed issues for similar_text seem to be only an issue for long sections of text (&gt;20000 chars).<br><br>I found a huge performance improvement in my application by just testing if the string to be tested was less than 20000 chars before calling similar_text.<br><br>20000+ took 3-5 secs to process, anything else (10000 and below) took a fraction of a second.<br>Fortunately for me, there was only a handful of instances with &gt;20000 chars which I couldn&apos;t get a comparison % for.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you have reserved names in a database that you don&apos;t want others to use, i find this to work pretty good. 
<br>I added strtoupper to the variables to validate typing only. Taking case into consideration will decrease similarity. 
<br>
<br><span class="default">&lt;?php
<br>$query </span><span class="keyword">= </span><span class="default">mysql_query</span><span class="keyword">(</span><span class="string">&quot;select * from </span><span class="default">$table</span><span class="string">&quot;</span><span class="keyword">) or die(</span><span class="string">&quot;Query failed&quot;</span><span class="keyword">);
<br>
<br>while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysql_fetch_array</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">similar_text</span><span class="keyword">(</span><span class="default">strtoupper</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;name&apos;</span><span class="keyword">]), </span><span class="default">strtoupper</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;reserved&apos;</span><span class="keyword">]), </span><span class="default">$similarity_pst</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">number_format</span><span class="keyword">(</span><span class="default">$similarity_pst</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">) &gt; </span><span class="default">90</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$too_similar </span><span class="keyword">= </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;reserved&apos;</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;The name you entered is too similar the reserved name &amp;quot;&quot;</span><span class="keyword">.</span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;reserved&apos;</span><span class="keyword">].</span><span class="string">&quot;&amp;quot;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.similar-text.php)

**[To root](/README.md)**