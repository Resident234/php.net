# get_meta_tags




<div class="phpcode"><span class="html">
This regex gets meta tags independent of sequence by capturing inside a lookahead.<br>Further uses the branch reset feature for different quote styles of values.<br>The pattern can be tested here: <a href="https://regex101.com/r/oE4oU9/1" rel="nofollow" target="_blank">https://regex101.com/r/oE4oU9/1</a><br><br><span class="default">&lt;?PHP<br><br></span><span class="keyword">function </span><span class="default">getMetaTags</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">)<br>{<br>&#xA0; </span><span class="default">$pattern </span><span class="keyword">= </span><span class="string">&apos;<br>&#xA0; ~&lt;\s*meta\s<br><br>&#xA0; # using lookahead to capture type to $1<br>&#xA0; &#xA0; (?=[^&gt;]*?<br>&#xA0; &#xA0; \b(?:name|property|http-equiv)\s*=\s*<br>&#xA0; &#xA0; (?|&quot;\s*([^&quot;]*?)\s*&quot;|\&apos;\s*([^\&apos;]*?)\s*\&apos;|<br>&#xA0; &#xA0; ([^&quot;\&apos;&gt;]*?)(?=\s*/?\s*&gt;|\s\w+\s*=))<br>&#xA0; )<br><br>&#xA0; # capture content to $2<br>&#xA0; [^&gt;]*?\bcontent\s*=\s*<br>&#xA0; &#xA0; (?|&quot;\s*([^&quot;]*?)\s*&quot;|\&apos;\s*([^\&apos;]*?)\s*\&apos;|<br>&#xA0; &#xA0; ([^&quot;\&apos;&gt;]*?)(?=\s*/?\s*&gt;|\s\w+\s*=))<br>&#xA0; [^&gt;]*&gt;<br><br>&#xA0; ~ix&apos;</span><span class="keyword">;<br>&#xA0; <br>&#xA0; if(</span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">, </span><span class="default">$out</span><span class="keyword">))<br>&#xA0; &#xA0; return </span><span class="default">array_combine</span><span class="keyword">(</span><span class="default">$out</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$out</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]);<br>&#xA0; return array();<br>}<br><br></span><span class="comment">// usage<br></span><span class="default">$meta_tags </span><span class="keyword">= </span><span class="default">getMetaTags</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-meta-tags.php)

**[⬆ to root](/)**