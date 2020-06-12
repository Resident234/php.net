# number_format




<div class="phpcode"><span class="html">
It&apos;s not explicitly documented; number_format also rounds:<br><br><span class="default">&lt;?php<br>$numbers </span><span class="keyword">= array(</span><span class="default">0.001</span><span class="keyword">, </span><span class="default">0.002</span><span class="keyword">, </span><span class="default">0.003</span><span class="keyword">, </span><span class="default">0.004</span><span class="keyword">, </span><span class="default">0.005</span><span class="keyword">, </span><span class="default">0.006</span><span class="keyword">, </span><span class="default">0.007</span><span class="keyword">, </span><span class="default">0.008</span><span class="keyword">, </span><span class="default">0.009</span><span class="keyword">);<br>foreach (</span><span class="default">$numbers </span><span class="keyword">as </span><span class="default">$number</span><span class="keyword">)<br>&#xA0; &#xA0; print </span><span class="default">$number</span><span class="keyword">.</span><span class="string">&quot;-&gt;&quot;</span><span class="keyword">.</span><span class="default">number_format</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;.&apos;</span><span class="keyword">, </span><span class="string">&apos;,&apos;</span><span class="keyword">).</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>0.001-&gt;0.00<br>0.002-&gt;0.00<br>0.003-&gt;0.00<br>0.004-&gt;0.00<br>0.005-&gt;0.01<br>0.006-&gt;0.01<br>0.007-&gt;0.01<br>0.008-&gt;0.01<br>0.009-&gt;0.01</span>
</div>
  

#


<div class="phpcode"><span class="html">
I ran across an issue where I wanted to keep the entered precision of a real value, without arbitrarily rounding off what the user had submitted.<br><br>I figured it out with a quick explode on the number before formatting. I could then format either side of the decimal.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">number_format_unlimited_precision</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">,</span><span class="default">$decimal </span><span class="keyword">= </span><span class="string">&apos;.&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$broken_number </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$decimal</span><span class="keyword">,</span><span class="default">$number</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">number_format</span><span class="keyword">(</span><span class="default">$broken_number</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]).</span><span class="default">$decimal</span><span class="keyword">.</span><span class="default">$broken_number</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Outputs a human readable number.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">#&#xA0; &#xA0; Output easy-to-read numbers<br>&#xA0; &#xA0; #&#xA0; &#xA0; by james at bandit.co.nz<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">bd_nice_number</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// first strip any formatting;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$n </span><span class="keyword">= (</span><span class="default">0</span><span class="keyword">+</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;,&quot;</span><span class="keyword">,</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">$n</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// is this a number?<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(!</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">)) return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// now filter it;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$n</span><span class="keyword">&gt;</span><span class="default">1000000000000</span><span class="keyword">) return </span><span class="default">round</span><span class="keyword">((</span><span class="default">$n</span><span class="keyword">/</span><span class="default">1000000000000</span><span class="keyword">),</span><span class="default">1</span><span class="keyword">).</span><span class="string">&apos; trillion&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else if(</span><span class="default">$n</span><span class="keyword">&gt;</span><span class="default">1000000000</span><span class="keyword">) return </span><span class="default">round</span><span class="keyword">((</span><span class="default">$n</span><span class="keyword">/</span><span class="default">1000000000</span><span class="keyword">),</span><span class="default">1</span><span class="keyword">).</span><span class="string">&apos; billion&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else if(</span><span class="default">$n</span><span class="keyword">&gt;</span><span class="default">1000000</span><span class="keyword">) return </span><span class="default">round</span><span class="keyword">((</span><span class="default">$n</span><span class="keyword">/</span><span class="default">1000000</span><span class="keyword">),</span><span class="default">1</span><span class="keyword">).</span><span class="string">&apos; million&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; else if(</span><span class="default">$n</span><span class="keyword">&gt;</span><span class="default">1000</span><span class="keyword">) return </span><span class="default">round</span><span class="keyword">((</span><span class="default">$n</span><span class="keyword">/</span><span class="default">1000</span><span class="keyword">),</span><span class="default">1</span><span class="keyword">).</span><span class="string">&apos; thousand&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">number_format</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">);<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Outputs:<br><br>247,704,360 -&gt; 247.7 million<br>866,965,260,000 -&gt; 867 billion</span>
</div>
  

#


<div class="phpcode"><span class="html">
For Zero fill - just use the sprintf() function<br><br>$pr_id = 1;<br>$pr_id = sprintf(&quot;%03d&quot;, $pr_id);<br>echo $pr_id;<br><br>//outputs 001<br>-----------------<br><br>$pr_id = 10;<br>$pr_id = sprintf(&quot;%03d&quot;, $pr_id);<br>echo $pr_id;<br><br>//outputs 010<br>-----------------<br><br>You can change %03d to %04d, etc.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just an observation:<br>The number_format rounds the value of the variable.<br><br>$val1 = 1.233;<br>$val2 = 1.235;<br>$val3 = 1.237;<br><br>echo number_format($val1,2,&quot;,&quot;,&quot;.&quot;); // returns: 1,23<br>echo number_format($val2,2,&quot;,&quot;,&quot;.&quot;); // returns: 1,24<br>echo number_format($val3,2,&quot;,&quot;,&quot;.&quot;); // returns: 1,24</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.number-format.php)

**[⬆ to root](/)**