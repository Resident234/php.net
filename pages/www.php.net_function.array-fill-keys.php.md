# array_fill_keys




<div class="phpcode"><span class="html">
<span class="default">&lt;?php
<br>$a </span><span class="keyword">= array(</span><span class="string">&quot;1&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_fill_keys</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="string">&quot;test&quot;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>array(1) {
<br>&#xA0; [1]=&gt;
<br>&#xA0; string(4) &quot;test&quot;
<br>}
<br>
<br>now string key &quot;1&quot; become an integer value 1, be careful.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If an associative array is used as the second parameter of array_fill_keys, then the associative array will be appended in all the values of the first array.
<br>e.g.
<br><span class="default">&lt;?php
<br>$array1 </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="string">&quot;a&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;first&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&quot;b&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;second&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&quot;c&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;something&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&quot;red&quot;
<br></span><span class="keyword">);
<br>
<br></span><span class="default">$array2 </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="string">&quot;a&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;first&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&quot;b&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;something&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&quot;letsc&quot;
<br></span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_fill_keys</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>The output will be
<br>Array(
<br>&#xA0; &#xA0; [first] =&gt; Array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; first,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; something,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; letsc
<br>&#xA0; &#xA0; ),
<br>&#xA0; &#xA0; [second] =&gt; Array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; first,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; something,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; letsc
<br>&#xA0; &#xA0; ),
<br>&#xA0; &#xA0; [something] =&gt; Array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; first,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; something,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; letsc
<br>&#xA0; &#xA0; ),
<br>&#xA0; &#xA0; [red] =&gt; Array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; first,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; something,
<br>&#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; letsc
<br>&#xA0; &#xA0; )
<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-fill-keys.php)

**[To root](/README.md)**