# eval




<div class="phpcode"><span class="html">
Kepp the following Quote in mind:<br><br>If eval() is the answer, you&apos;re almost certainly asking the<br>wrong question. -- Rasmus Lerdorf, BDFL of PHP</span>
</div>
  

#


<div class="phpcode"><span class="html">
Inception with eval()<br><br>&lt;pre&gt;<br>Inception Start:<br><span class="default">&lt;?php<br></span><span class="keyword">eval(</span><span class="string">&quot;echo &apos;Inception lvl 1...\n&apos;; eval(&apos;echo \&quot;Inception lvl 2...\n\&quot;; eval(\&quot;echo \&apos;Inception lvl 3...\n\&apos;; eval(\&apos;echo \\\&quot;Limbo!\\\&quot;;\&apos;);\&quot;);&apos;);&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
At least in PHP 7.1+, eval() terminates the script if the evaluated code generate a fatal error. For example:<br><span class="default">&lt;?php<br></span><span class="keyword">@eval(</span><span class="string">&apos;$content = (100 - );&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>(Even if it is in the man, I&apos;m note sure it acted like this in 5.6, but whatever)<br>To catch it, I had to do:<br><span class="default">&lt;?php<br></span><span class="keyword">try {<br>&#xA0; &#xA0; eval(</span><span class="string">&apos;$content = (100 - );&apos;</span><span class="keyword">);<br>} catch (</span><span class="default">Throwable $t</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>This is the only way I found to catch the error and hide the fact there was one.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to allow math input and make sure that the input is proper mathematics and not some hacking code, you can try this:<br><br><span class="default">&lt;?php<br><br>$test </span><span class="keyword">= </span><span class="string">&apos;2+3*pi&apos;</span><span class="keyword">;<br><br></span><span class="comment">// Remove whitespaces<br></span><span class="default">$test </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\s+/&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$test</span><span class="keyword">);<br><br></span><span class="default">$number </span><span class="keyword">= </span><span class="string">&apos;(?:\d+(?:[,.]\d+)?|pi|&#x3C0;)&apos;</span><span class="keyword">; </span><span class="comment">// What is a number<br></span><span class="default">$functions </span><span class="keyword">= </span><span class="string">&apos;(?:sinh?|cosh?|tanh?|abs|acosh?|asinh?|atanh?|exp|log10|deg2rad|rad2deg|sqrt|ceil|floor|round)&apos;</span><span class="keyword">; </span><span class="comment">// Allowed PHP functions<br></span><span class="default">$operators </span><span class="keyword">= </span><span class="string">&apos;[+\/*\^%-]&apos;</span><span class="keyword">; </span><span class="comment">// Allowed math operators<br></span><span class="default">$regexp </span><span class="keyword">= </span><span class="string">&apos;/^((&apos;</span><span class="keyword">.</span><span class="default">$number</span><span class="keyword">.</span><span class="string">&apos;|&apos;</span><span class="keyword">.</span><span class="default">$functions</span><span class="keyword">.</span><span class="string">&apos;\s*\((?1)+\)|\((?1)+\))(?:&apos;</span><span class="keyword">.</span><span class="default">$operators</span><span class="keyword">.</span><span class="string">&apos;(?2))?)+$/&apos;</span><span class="keyword">; </span><span class="comment">// Final regexp, heavily using recursive patterns<br><br></span><span class="keyword">if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="default">$regexp</span><span class="keyword">, </span><span class="default">$q</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; </span><span class="default">$test </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;!pi|&#x3C0;!&apos;</span><span class="keyword">, </span><span class="string">&apos;pi()&apos;</span><span class="keyword">, </span><span class="default">$test</span><span class="keyword">); </span><span class="comment">// Replace pi with pi function<br>&#xA0; &#xA0; </span><span class="keyword">eval(</span><span class="string">&apos;$result = &apos;</span><span class="keyword">.</span><span class="default">$test</span><span class="keyword">.</span><span class="string">&apos;;&apos;</span><span class="keyword">);<br>}<br>else<br>{<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>I can&apos;t guarantee you absolutely that this will block every possible malicious code nor that it will block malformed code, but that&apos;s better than the matheval function below which will allow malformed code like &apos;2+2+&apos; which will throw an error.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.eval.php)

**[⬆ to root](/)**