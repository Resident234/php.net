# array_flip




<div class="phpcode"><span class="html">
I find this function vey useful when you have a big array and you want to know if a given value is in the array. in_array in fact becomes quite slow in such a case, but you can flip the big array and then use isset to obtain the same result in a much faster way.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function is useful when parsing a CSV file with a heading column, but the columns might vary in order or presence:<br><br><span class="default">&lt;?php<br><br>$f </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;file.csv&quot;</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">);<br><br></span><span class="comment">/* Take the first line (the header) into an array, then flip it<br>so that the keys are the column name, and values are the<br>column index. */<br></span><span class="default">$cols </span><span class="keyword">= </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">));<br><br>while (</span><span class="default">$line </span><span class="keyword">= </span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; </span><span class="comment">// Now we can reference CSV columns like so:<br>&#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">= </span><span class="default">$line</span><span class="keyword">[</span><span class="default">$cols</span><span class="keyword">[</span><span class="string">&apos;OrderStatus&apos;</span><span class="keyword">]];<br>}<br><br></span><span class="default">?&gt;<br></span><br>I find this better than referencing the numerical array index.</span>
</div>
  

#


<div class="phpcode"><span class="html">
array_flip() does not retain the data type of values, when converting them into keys. :( <br><br><span class="default">&lt;?php&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>$arr </span><span class="keyword">= array(</span><span class="string">&apos;one&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;1&apos;</span><span class="keyword">, </span><span class="string">&apos;two&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;2&apos;</span><span class="keyword">, </span><span class="string">&apos;three&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;3&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br></span><span class="default">$arr2 </span><span class="keyword">= </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$arr2</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This code outputs this:<br>array(3) {<br>&#xA0; [&quot;one&quot;]=&gt;<br>&#xA0; string(1) &quot;1&quot;<br>&#xA0; [&quot;two&quot;]=&gt;<br>&#xA0; string(1) &quot;2&quot;<br>&#xA0; [&quot;three&quot;]=&gt;<br>&#xA0; string(1) &quot;3&quot;<br>}<br>array(3) {<br>&#xA0; [1]=&gt;<br>&#xA0; string(3) &quot;one&quot;<br>&#xA0; [2]=&gt;<br>&#xA0; string(3) &quot;two&quot;<br>&#xA0; [3]=&gt;<br>&#xA0; string(5) &quot;three&quot;<br>}<br><br>It is valid expectation that string values &quot;1&quot;, &quot;2&quot; and &quot;3&quot; would become string keys &quot;1&quot;, &quot;2&quot; and &quot;3&quot;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-flip.php)

**[To root](/README.md)**