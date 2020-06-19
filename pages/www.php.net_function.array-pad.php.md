# array_pad




<div class="phpcode"><span class="html">
Beware, if you try to pad an associative array using numeric keys, your keys will be re-numbered.
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= array(</span><span class="string">&apos;size&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;large&apos;</span><span class="keyword">, </span><span class="string">&apos;number&apos;</span><span class="keyword">=&gt;</span><span class="default">20</span><span class="keyword">, </span><span class="string">&apos;color&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;red&apos;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_pad</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">, </span><span class="string">&apos;foo&apos;</span><span class="keyword">));
<br>
<br></span><span class="comment">// use timestamps as keys
<br></span><span class="default">$b </span><span class="keyword">= array(</span><span class="default">1229600459</span><span class="keyword">=&gt;</span><span class="string">&apos;large&apos;</span><span class="keyword">, </span><span class="default">1229604787</span><span class="keyword">=&gt;</span><span class="default">20</span><span class="keyword">, </span><span class="default">1229609459</span><span class="keyword">=&gt;</span><span class="string">&apos;red&apos;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_pad</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">, </span><span class="string">&apos;foo&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>yields this:
<br>------------------
<br>Array
<br>(
<br>&#xA0; &#xA0; [size] =&gt; large
<br>&#xA0; &#xA0; [number] =&gt; 20
<br>&#xA0; &#xA0; [color] =&gt; red
<br>)
<br>Array
<br>(
<br>&#xA0; &#xA0; [size] =&gt; large
<br>&#xA0; &#xA0; [number] =&gt; 20
<br>&#xA0; &#xA0; [color] =&gt; red
<br>&#xA0; &#xA0; [0] =&gt; foo
<br>&#xA0; &#xA0; [1] =&gt; foo
<br>)
<br>Array
<br>(
<br>&#xA0; &#xA0; [1229600459] =&gt; large
<br>&#xA0; &#xA0; [1229604787] =&gt; 20
<br>&#xA0; &#xA0; [1229609459] =&gt; red
<br>)
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; large
<br>&#xA0; &#xA0; [1] =&gt; 20
<br>&#xA0; &#xA0; [2] =&gt; red
<br>&#xA0; &#xA0; [3] =&gt; foo
<br>&#xA0; &#xA0; [4] =&gt; foo
<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-pad.php)

**[To root](/README.md)**