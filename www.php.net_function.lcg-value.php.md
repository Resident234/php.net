# lcg_value




<div class="phpcode"><span class="html">
An elegant way to return random float between two numbers:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">random_float </span><span class="keyword">(</span><span class="default">$min</span><span class="keyword">,</span><span class="default">$max</span><span class="keyword">) {<br>&#xA0;&#xA0; return (</span><span class="default">$min</span><span class="keyword">+</span><span class="default">lcg_value</span><span class="keyword">()*(</span><span class="default">abs</span><span class="keyword">(</span><span class="default">$max</span><span class="keyword">-</span><span class="default">$min</span><span class="keyword">)));<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Choose your weapon:<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">mt_randf</span><span class="keyword">(</span><span class="default">$min</span><span class="keyword">, </span><span class="default">$max</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">$min </span><span class="keyword">+ </span><span class="default">abs</span><span class="keyword">(</span><span class="default">$max </span><span class="keyword">- </span><span class="default">$min</span><span class="keyword">) * </span><span class="default">mt_rand</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">mt_getrandmax</span><span class="keyword">())/</span><span class="default">mt_getrandmax</span><span class="keyword">(); <br>}<br>function </span><span class="default">lcg_randf</span><span class="keyword">(</span><span class="default">$min</span><span class="keyword">, </span><span class="default">$max</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">$min </span><span class="keyword">+ </span><span class="default">lcg_value</span><span class="keyword">() * </span><span class="default">abs</span><span class="keyword">(</span><span class="default">$max </span><span class="keyword">- </span><span class="default">$min</span><span class="keyword">);<br>}<br>function </span><span class="default">randf</span><span class="keyword">(</span><span class="default">$min</span><span class="keyword">, </span><span class="default">$max</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">$min </span><span class="keyword">+ </span><span class="default">rand</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">,</span><span class="default">getrandmax</span><span class="keyword">()) / </span><span class="default">getrandmax</span><span class="keyword">() * </span><span class="default">abs</span><span class="keyword">(</span><span class="default">$max </span><span class="keyword">- </span><span class="default">$min</span><span class="keyword">);<br>}</span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.lcg-value.php)

**[⬆ to root](/)**