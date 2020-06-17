# Arithmetic Operators




<div class="phpcode"><span class="html">
The modulus operator is very poorly suited for such a simple operation as determining if an int is even or odd. On most common systems, modulus performs a division, which is a very slow operation.<br>A much better way to find if a number is even or odd is to use the bitwise &amp; operator.<br><br>e.g.<br><br>$is_odd = $x &amp; 1; //using and<br>$is_odd = $x % 2; //using modulus</span>
</div>
  

#


<div class="phpcode"><span class="html">
A very simple yet maybe not obvious use of the modulus (%) operator is to check if an integer is odd or even.<br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">if ((</span><span class="default">$a </span><span class="keyword">% </span><span class="default">2</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">)<br>&#xA0; { echo </span><span class="string">&quot;</span><span class="default">$a</span><span class="string"> is odd.&quot; </span><span class="keyword">;}<br>&#xA0; if ((</span><span class="default">$a </span><span class="keyword">% </span><span class="default">2</span><span class="keyword">) == </span><span class="default">0</span><span class="keyword">)<br>&#xA0; { echo </span><span class="string">&quot;</span><span class="default">$a</span><span class="string"> is even.&quot; </span><span class="keyword">;}<br></span><span class="default">?&gt;<br></span><br>This is nice when you want to make alternating-color rows on a table, or divs.<br><br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">10</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {<br>&#xA0; &#xA0; if((</span><span class="default">$i </span><span class="keyword">% </span><span class="default">2</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">)&#xA0; </span><span class="comment">//odd<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">{echo </span><span class="string">&quot;&lt;div class=\&quot;dark\&quot;&gt;</span><span class="default">$i</span><span class="string">&lt;/div&gt;&quot;</span><span class="keyword">;}<br>&#xA0; &#xA0; else&#xA0;&#xA0; </span><span class="comment">//even<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">{echo </span><span class="string">&quot;&lt;div class=\&quot;light\&quot;&gt;</span><span class="default">$i</span><span class="string">&lt;/div&gt;&quot;</span><span class="keyword">;}<br>&#xA0;&#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that operator % (modulus) works just with integers (between -214748348 and 2147483647) while fmod() works with short and large numbers.<br><br>Modulus with non integer numbers will give unpredictable results.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.arithmetic.php)

**[To root](/README.md)**