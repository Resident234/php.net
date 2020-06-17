# version_compare




<div class="phpcode"><span class="html">
[editors note]
<br>snipbit fixed after comment from Matt Mullenweg
<br>
<br>--jm
<br>[/editors note]
<br>
<br>so in a nutshell... I believe it works best like this:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if (</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&quot;4.3.0&quot;</span><span class="keyword">, </span><span class="string">&quot;&gt;=&quot;</span><span class="keyword">)) {
<br>&#xA0; </span><span class="comment">// you&apos;re on 4.3.0 or later
<br></span><span class="keyword">} else {
<br>&#xA0; </span><span class="comment">// you&apos;re not
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Since this function considers 1 &lt; 1.0 &lt; 1.0.0, others might find this function useful (which considers 1 == 1.0):
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">//Compare two sets of versions, where major/minor/etc. releases are separated by dots.
<br>//Returns 0 if both are equal, 1 if A &gt; B, and -1 if B &lt; A.
<br></span><span class="keyword">function </span><span class="default">version_compare2</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$a </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="string">&quot;.0&quot;</span><span class="keyword">)); </span><span class="comment">//Split version into pieces and remove trailing .0
<br>&#xA0; &#xA0; </span><span class="default">$b </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="string">&quot;.0&quot;</span><span class="keyword">)); </span><span class="comment">//Split version into pieces and remove trailing .0
<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$a </span><span class="keyword">as </span><span class="default">$depth </span><span class="keyword">=&gt; </span><span class="default">$aVal</span><span class="keyword">)
<br>&#xA0; &#xA0; { </span><span class="comment">//Iterate over each piece of A
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (isset(</span><span class="default">$b</span><span class="keyword">[</span><span class="default">$depth</span><span class="keyword">]))
<br>&#xA0; &#xA0; &#xA0; &#xA0; { </span><span class="comment">//If B matches A to this depth, compare the values
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$aVal </span><span class="keyword">&gt; </span><span class="default">$b</span><span class="keyword">[</span><span class="default">$depth</span><span class="keyword">]) return </span><span class="default">1</span><span class="keyword">; </span><span class="comment">//Return A &gt; B
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">else if (</span><span class="default">$aVal </span><span class="keyword">&lt; </span><span class="default">$b</span><span class="keyword">[</span><span class="default">$depth</span><span class="keyword">]) return -</span><span class="default">1</span><span class="keyword">; </span><span class="comment">//Return B &gt; A
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //An equal result is inconclusive at this point
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; { </span><span class="comment">//If B does not match A to this depth, then A comes after B in sort order
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">1</span><span class="keyword">; </span><span class="comment">//so return A &gt; B
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">//At this point, we know that to the depth that A and B extend to, they are equivalent.
<br>&#xA0; &#xA0; //Either the loop ended because A is shorter than B, or both are equal.
<br>&#xA0; &#xA0; </span><span class="keyword">return (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">) &lt; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">)) ? -</span><span class="default">1 </span><span class="keyword">: </span><span class="default">0</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.version-compare.php)

**[To root](/README.md)**