# floor




<div class="phpcode"><span class="html">
I believe this behavior of the floor function was intended.&#xA0; Note that it says &quot;the next lowest integer&quot;.&#xA0; -1 is &quot;higher&quot; than -1.6.&#xA0; As in, -1 is logically greater than -1.6.&#xA0; To go lower the floor function would go to -2 which is logically less than -1.6.<br><br>Floor isn&apos;t trying to give you the number closest to zero, it&apos;s giving you the lowest bounding integer of a float.<br><br>In reply to Glen who commented:<br> Glen<br>01-Dec-2007 04:22<br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">echo </span><span class="default">floor</span><span class="keyword">(</span><span class="default">1.6</span><span class="keyword">);&#xA0; </span><span class="comment">// will output &quot;1&quot;<br>&#xA0; </span><span class="keyword">echo </span><span class="default">floor</span><span class="keyword">(-</span><span class="default">1.6</span><span class="keyword">); </span><span class="comment">// will output &quot;-2&quot;<br></span><span class="default">?&gt;<br></span><br>instead use intval (seems to work v5.1.6):<br><br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">echo </span><span class="default">intval</span><span class="keyword">(</span><span class="default">1.6</span><span class="keyword">);&#xA0; </span><span class="comment">// will output &quot;1&quot;<br>&#xA0; </span><span class="keyword">echo </span><span class="default">intval</span><span class="keyword">(-</span><span class="default">1.6</span><span class="keyword">); </span><span class="comment">// will output &quot;-1&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I use this function to floor with decimals:<br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">floordec</span><span class="keyword">(</span><span class="default">$zahl</span><span class="keyword">,</span><span class="default">$decimals</span><span class="keyword">=</span><span class="default">2</span><span class="keyword">){&#xA0; &#xA0; <br>&#xA0; &#xA0;&#xA0; return </span><span class="default">floor</span><span class="keyword">(</span><span class="default">$zahl</span><span class="keyword">*</span><span class="default">pow</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">,</span><span class="default">$decimals</span><span class="keyword">))/</span><span class="default">pow</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">,</span><span class="default">$decimals</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A correction to the funcion floor_dec from the user &quot;php is the best&quot;.<br>If the number is 0.05999 it returns 0.59 because the zero at left position is deleted.<br>I just added a &apos;1&apos; and after the floor or ceil call remove with a substr.<br>Hope it helps.<br><br>function floor_dec($number,$precision = 2,$separator = &apos;.&apos;) {<br>&#xA0; $numberpart=explode($separator,$number);<br>&#xA0; $numberpart[1]=substr_replace($numberpart[1],$separator,$precision,0);<br>&#xA0; if($numberpart[0]&gt;=0) {<br>&#xA0; &#xA0; $numberpart[1]=substr(floor(&apos;1&apos;.$numberpart[1]),1);<br>&#xA0; } else {<br>&#xA0; &#xA0; $numberpart[1]=substr(ceil(&apos;1&apos;.$numberpart[1]),1);<br>&#xA0; }<br>&#xA0; $ceil_number= array($numberpart[0],$numberpart[1]);<br>&#xA0; return implode($separator,$ceil_number);<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
But, if you want the number closest to zero, you could use this:<br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">if(</span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">floor</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">);<br>&#xA0; } else {<br>&#xA0; &#xA0; </span><span class="default">ceil</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">);<br>&#xA0; }<br></span><span class="default">?&gt;<br></span><br>-benrr101</span>
</div>
  

#


<div class="phpcode"><span class="html">
Have solved a &quot;price problem&quot;:
<br>
<br><span class="default">&lt;?php
<br>$peny </span><span class="keyword">= </span><span class="default">floor</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">-&gt;</span><span class="default">price</span><span class="keyword">*</span><span class="default">1000</span><span class="keyword">) - </span><span class="default">floor</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">-&gt;</span><span class="default">price</span><span class="keyword">)*</span><span class="default">1000</span><span class="keyword">)/</span><span class="default">10</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.floor.php)

**[⬆ to root](/)**