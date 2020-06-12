# ImagickDraw::annotation




<div class="phpcode"><span class="html">
may help someone...
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="comment">/**
<br>&#xA0; &#xA0;&#xA0; * Split the given text into rows fitting the given maxWidth
<br>&#xA0; &#xA0;&#xA0; *
<br>&#xA0; &#xA0;&#xA0; * @param unknown_type $draw
<br>&#xA0; &#xA0;&#xA0; * @param unknown_type $text
<br>&#xA0; &#xA0;&#xA0; * @param unknown_type $maxWidth
<br>&#xA0; &#xA0;&#xA0; * @return array
<br>&#xA0; &#xA0;&#xA0; */
<br>&#xA0; &#xA0; </span><span class="keyword">private function </span><span class="default">getTextRows</span><span class="keyword">(</span><span class="default">$draw</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">, </span><span class="default">$maxWidth</span><span class="keyword">)
<br>&#xA0; &#xA0; {&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$words </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot; &quot;</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lines </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; while (</span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">)) 
<br>&#xA0; &#xA0; &#xA0; &#xA0; {</span><span class="comment">//as long as there are words
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; do
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {</span><span class="comment">//append words to line until the fit in size
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$line </span><span class="keyword">!= </span><span class="string">&quot;&quot;</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line </span><span class="keyword">.= </span><span class="string">&quot; &quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line </span><span class="keyword">.= </span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if((</span><span class="default">$i</span><span class="keyword">) == </span><span class="default">count</span><span class="keyword">(</span><span class="default">$words</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;</span><span class="comment">//last word -&gt; break
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//messure size of line + next word
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$linePreview </span><span class="keyword">= </span><span class="default">$line</span><span class="keyword">.</span><span class="string">&quot; &quot;</span><span class="keyword">.</span><span class="default">$words</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$metrics </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">canvas</span><span class="keyword">-&gt;</span><span class="default">queryFontMetrics</span><span class="keyword">(</span><span class="default">$draw</span><span class="keyword">, </span><span class="default">$linePreview</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//echo $line.&quot;($i)&quot;.$metrics[&quot;textWidth&quot;].&quot;:&quot;.$maxWidth.&quot;&lt;br&gt;&quot;;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}while(</span><span class="default">$metrics</span><span class="keyword">[</span><span class="string">&quot;textWidth&quot;</span><span class="keyword">] &lt;= </span><span class="default">$maxWidth</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//echo &quot;&lt;hr&gt;&quot;.$line.&quot;&lt;br&gt;&quot;;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lines</span><span class="keyword">[] = </span><span class="default">$line</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//var_export($lines);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$lines</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagickdraw.annotation.php)

**[⬆ to root](/)**