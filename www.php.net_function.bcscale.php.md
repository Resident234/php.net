# bcscale




<div class="phpcode"><span class="html">
Executing bcsacle() will change the scale value of fpm.conf, not only the current process.</span>
</div>
  

#


<div class="phpcode"><span class="html">
These functions DO NOT round off your values. No arbitrary precision libraries do it this way. It stops calculating after reaching scale of decimal places, which mean that your value is cut off after scale number of digits, not rounded. To do the rounding use something like this:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">bcround</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="default">$scale</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fix </span><span class="keyword">= </span><span class="string">&quot;5&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">$scale</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">++) </span><span class="default">$fix</span><span class="keyword">=</span><span class="string">&quot;0</span><span class="default">$fix</span><span class="string">&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$number </span><span class="keyword">= </span><span class="default">bcadd</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="string">&quot;0.</span><span class="default">$fix</span><span class="string">&quot;</span><span class="keyword">, </span><span class="default">$scale</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return&#xA0; &#xA0; </span><span class="default">bcdiv</span><span class="keyword">(</span><span class="default">$number</span><span class="keyword">, </span><span class="string">&quot;1.0&quot;</span><span class="keyword">,&#xA0; &#xA0; </span><span class="default">$scale</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.bcscale.php)

**[⬆ to root](/)**