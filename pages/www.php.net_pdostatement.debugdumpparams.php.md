# PDOStatement::debugDumpParams




<div class="phpcode"><span class="html">
This function doesn&apos;t print parameter values despite the documentation says it does. See <a href="https://bugs.php.net/bug.php?id=52384" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=52384</a> (filed back in 2010).</span>
</div>
  

#


<div class="phpcode"><span class="html">
As noted, this doesn&#x2019;t actually simply print the prepared statement with data to be executed.<br><br>For trouble shooting purposes, I find the following useful:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">parms</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">,</span><span class="default">$data</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$indexed</span><span class="keyword">=</span><span class="default">$data</span><span class="keyword">==</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$data </span><span class="keyword">as </span><span class="default">$k</span><span class="keyword">=&gt;</span><span class="default">$v</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">)) </span><span class="default">$v</span><span class="keyword">=</span><span class="string">&quot;&apos;</span><span class="default">$v</span><span class="string">&apos;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$indexed</span><span class="keyword">) </span><span class="default">$string</span><span class="keyword">=</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\?/&apos;</span><span class="keyword">,</span><span class="default">$v</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else </span><span class="default">$string</span><span class="keyword">=</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;:</span><span class="default">$k</span><span class="string">&quot;</span><span class="keyword">,</span><span class="default">$v</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$string</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">//&#xA0; &#xA0; Index Parameters<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string</span><span class="keyword">=</span><span class="string">&apos;INSERT INTO stuff(name,value) VALUES (?,?)&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data</span><span class="keyword">=array(</span><span class="string">&apos;Fred&apos;</span><span class="keyword">,</span><span class="default">23</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">//&#xA0; &#xA0; Named Parameters<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$string</span><span class="keyword">=</span><span class="string">&apos;INSERT INTO stuff(name,value) VALUES (:name,:value)&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data</span><span class="keyword">=array(</span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;Fred&apos;</span><span class="keyword">,</span><span class="string">&apos;value&apos;</span><span class="keyword">=&gt;</span><span class="default">23</span><span class="keyword">);<br><br>&#xA0; &#xA0; print </span><span class="default">parms</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">,</span><span class="default">$data</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.debugdumpparams.php)

**[To root](/README.md)**