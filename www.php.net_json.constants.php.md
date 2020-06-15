# Predefined Constants




<div class="phpcode"><span class="html">
To get a really clean json string use these three constants like so:<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= [</span><span class="string">&apos;&#x20AC;&apos;</span><span class="keyword">, </span><span class="string">&apos;<a href="http://example.com/some/cool/page" rel="nofollow" target="_blank">http://example.com/some/cool/page</a>&apos;</span><span class="keyword">, </span><span class="string">&apos;337&apos;</span><span class="keyword">];<br></span><span class="default">$bad&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">); <br></span><span class="default">$good&#xA0; </span><span class="keyword">= </span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">,&#xA0; </span><span class="default">JSON_UNESCAPED_UNICODE </span><span class="keyword">| </span><span class="default">JSON_UNESCAPED_SLASHES </span><span class="keyword">| </span><span class="default">JSON_NUMERIC_CHECK</span><span class="keyword">);<br><br></span><span class="comment">// $bad would be&#xA0; [&quot;\u20ac&quot;,&quot;http:\/\/example.com\/some\/cool\/page&quot;,&quot;337&quot;]<br>// $good would be [&quot;&#x20AC;&quot;,&quot;<a href="http://example.com/some/cool/page" rel="nofollow" target="_blank">http://example.com/some/cool/page</a>&quot;,337]<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you curious of the numeric values of the constants, as of JSON 1.2.1, the constants have the following values (not that you should use the numbers directly):<br><br>JSON_HEX_TAG =&gt; 1<br>JSON_HEX_AMP =&gt; 2<br>JSON_HEX_APOS =&gt; 4<br>JSON_HEX_QUOT =&gt; 8<br>JSON_FORCE_OBJECT =&gt; 16<br>JSON_NUMERIC_CHECK =&gt; 32<br>JSON_UNESCAPED_SLASHES =&gt; 64<br>JSON_PRETTY_PRINT =&gt; 128<br>JSON_UNESCAPED_UNICODE =&gt; 256<br><br>JSON_ERROR_DEPTH =&gt; 1<br>JSON_ERROR_STATE_MISMATCH =&gt; 2<br>JSON_ERROR_CTRL_CHAR =&gt; 3<br><br>JSON_ERROR_SYNTAX =&gt; 4<br><br>JSON_ERROR_UTF8 =&gt; 5<br>JSON_OBJECT_AS_ARRAY =&gt; 1<br><br>JSON_BIGINT_AS_STRING =&gt; 2</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/json.constants.php)

**[To root](/README.md)**