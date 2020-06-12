# strrev




<div class="phpcode"><span class="html">
This function support utf-8 encoding, Human Language and Character Encoding Support:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">mb_strrev</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$r </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">); </span><span class="default">$i</span><span class="keyword">&gt;=</span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">--) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$r </span><span class="keyword">.= </span><span class="default">mb_substr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$r</span><span class="keyword">;<br>}<br><br>echo </span><span class="default">mb_strrev</span><span class="keyword">(</span><span class="string">&quot;&#x2606;&#x2764;world&quot;</span><span class="keyword">); </span><span class="comment">// echo &quot;dlrow&#x2764;&#x2606;&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strrev.php)

**[⬆ to root](/)**