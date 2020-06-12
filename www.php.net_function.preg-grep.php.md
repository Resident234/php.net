# preg_grep




<div class="phpcode"><span class="html">
A shorter way to run a match on the array&apos;s keys rather than the values:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">preg_grep_keys</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$input</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="default">array_intersect_key</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">, </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">preg_grep</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">), </span><span class="default">$flags</span><span class="keyword">)));<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Run a match on the array&apos;s keys rather than the values:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">preg_grep_keys</span><span class="keyword">( </span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$input</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">0 </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">= </span><span class="default">preg_grep</span><span class="keyword">( </span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">( </span><span class="default">$input </span><span class="keyword">), </span><span class="default">$flags </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$vals </span><span class="keyword">= array();<br>&#xA0; &#xA0; foreach ( </span><span class="default">$keys </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$vals</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$input</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$vals</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-grep.php)

**[⬆ to root](/)**