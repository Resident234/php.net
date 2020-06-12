# floatval




<div class="phpcode"><span class="html">
This function takes the last comma or dot (if any) to make a clean float, ignoring thousand separator, currency or any other letter :<br><br>function tofloat($num) {<br>&#xA0; &#xA0; $dotPos = strrpos($num, &apos;.&apos;);<br>&#xA0; &#xA0; $commaPos = strrpos($num, &apos;,&apos;);<br>&#xA0; &#xA0; $sep = (($dotPos &gt; $commaPos) &amp;&amp; $dotPos) ? $dotPos : <br>&#xA0; &#xA0; &#xA0; &#xA0; ((($commaPos &gt; $dotPos) &amp;&amp; $commaPos) ? $commaPos : false);<br>&#xA0;&#xA0; <br>&#xA0; &#xA0; if (!$sep) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return floatval(preg_replace(&quot;/[^0-9]/&quot;, &quot;&quot;, $num));<br>&#xA0; &#xA0; } <br><br>&#xA0; &#xA0; return floatval(<br>&#xA0; &#xA0; &#xA0; &#xA0; preg_replace(&quot;/[^0-9]/&quot;, &quot;&quot;, substr($num, 0, $sep)) . &apos;.&apos; .<br>&#xA0; &#xA0; &#xA0; &#xA0; preg_replace(&quot;/[^0-9]/&quot;, &quot;&quot;, substr($num, $sep+1, strlen($num)))<br>&#xA0; &#xA0; );<br>}<br><br>$num = &apos;1.999,369&#x20AC;&apos;;<br>var_dump(tofloat($num)); // float(1999.369)<br>$otherNum = &apos;126,564,789.33 m&#xB2;&apos;;<br>var_dump(tofloat($otherNum)); // float(126564789.33)<br><br>Demo : <a href="http://codepad.org/NW4e9hQH" rel="nofollow" target="_blank">http://codepad.org/NW4e9hQH</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
you can also use typecasting instead of functions:<br><br>(float) $value;</span>
</div>
  

#


<div class="phpcode"><span class="html">
To view the very large and very small numbers (eg from a database DECIMAL), without displaying scientific notation, or leading zeros.<br><br>FR : Pour afficher les tr&#xE8;s grand et tr&#xE8;s petits nombres (ex. depuis une base de donn&#xE9;es DECIMAL), sans afficher la notation scientifique, ni les z&#xE9;ros non significatifs.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">floattostr</span><span class="keyword">( </span><span class="default">$val </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">preg_match</span><span class="keyword">( </span><span class="string">&quot;#^([\+\-]|)([0-9]*)(\.([0-9]*?)|)(0*)$#&quot;</span><span class="keyword">, </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">), </span><span class="default">$o </span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$o</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">].</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;%d&apos;</span><span class="keyword">,</span><span class="default">$o</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]).(</span><span class="default">$o</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]!=</span><span class="string">&apos;.&apos;</span><span class="keyword">?</span><span class="default">$o</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]:</span><span class="string">&apos;&apos;</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;0000000000000001&quot;</span><span class="keyword">);<br>echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;1.00000000000000&quot;</span><span class="keyword">);<br>echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;0.00000000001000&quot;</span><span class="keyword">);<br>echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;0000.00010000000&quot;</span><span class="keyword">);<br>echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;000000010000000000.00000000000010000000000&quot;</span><span class="keyword">);<br>echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;-0000000000000.1&quot;</span><span class="keyword">);<br>echo </span><span class="default">floattostr</span><span class="keyword">(</span><span class="string">&quot;-00000001.100000&quot;</span><span class="keyword">);<br><br></span><span class="comment">// result<br>// 1<br>// 1<br>// 0.00000000001<br>// 0.0001<br>// 10000000000.0000000000001<br>// -0.1<br>// -1.1<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Easier-to-grasp-function for the &apos;,&apos; problem.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">Getfloat</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">) {
<br>&#xA0; if(</span><span class="default">strstr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="string">&quot;,&quot;</span><span class="keyword">)) {
<br>&#xA0; &#xA0; </span><span class="default">$str </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="string">&quot;&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">); </span><span class="comment">// replace dots (thousand seps) with blancs
<br>&#xA0; &#xA0; </span><span class="default">$str </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;,&quot;</span><span class="keyword">, </span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">); </span><span class="comment">// replace &apos;,&apos; with &apos;.&apos;
<br>&#xA0; </span><span class="keyword">}
<br>&#xA0; 
<br>&#xA0; if(</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#([0-9\.]+)#&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">$match</span><span class="keyword">)) { </span><span class="comment">// search for number that may contain &apos;.&apos;
<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">floatval</span><span class="keyword">(</span><span class="default">$match</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);
<br>&#xA0; } else {
<br>&#xA0; &#xA0; return </span><span class="default">floatval</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">); </span><span class="comment">// take some last chances with floatval
<br>&#xA0; </span><span class="keyword">}
<br>}
<br>
<br>echo </span><span class="default">Getfloat</span><span class="keyword">(</span><span class="string">&quot;$ 19.332,35-&quot;</span><span class="keyword">); </span><span class="comment">// will print: 19332.35
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.floatval.php)

**[⬆ to root](/)**