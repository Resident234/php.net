# New features




<div class="phpcode"><span class="html">
<span class="default">&lt;?php <br></span><span class="keyword">class </span><span class="default">foo </span><span class="keyword">{ static </span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&apos;baz&apos;</span><span class="keyword">; }<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;foo&apos;</span><span class="keyword">::</span><span class="default">$bar</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>if &lt; php7.0<br><br>then we will receive a syntax error, unexpected &apos;::&apos; (T_PAAMAYIM_NEKUDOTAYIM) <br><br>but php7 returns string(3) &quot;baz&quot;<br><br>I think it&apos;s not documented anywhere</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration70.new-features.php)

**[To root](/README.md)**