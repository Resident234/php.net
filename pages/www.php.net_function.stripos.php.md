# stripos




<div class="phpcode"><span class="html">
I found myself needing to find the first position of multiple needles in one haystack.&#xA0; So I wrote this little function:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">multineedle_stripos</span><span class="keyword">(</span><span class="default">$haystack</span><span class="keyword">, </span><span class="default">$needles</span><span class="keyword">, </span><span class="default">$offset</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; foreach(</span><span class="default">$needles </span><span class="keyword">as </span><span class="default">$needle</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$found</span><span class="keyword">[</span><span class="default">$needle</span><span class="keyword">] = </span><span class="default">stripos</span><span class="keyword">(</span><span class="default">$haystack</span><span class="keyword">, </span><span class="default">$needle</span><span class="keyword">, </span><span class="default">$offset</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$found</span><span class="keyword">;<br>}<br><br></span><span class="comment">// It works as such:<br></span><span class="default">$haystack </span><span class="keyword">= </span><span class="string">&quot;The quick brown fox jumps over the lazy dog.&quot;</span><span class="keyword">;<br></span><span class="default">$needle </span><span class="keyword">= array(</span><span class="string">&quot;fox&quot;</span><span class="keyword">, </span><span class="string">&quot;dog&quot;</span><span class="keyword">, </span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="string">&quot;duck&quot;</span><span class="keyword">)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">multineedle_stripos</span><span class="keyword">(</span><span class="default">$haystack</span><span class="keyword">, </span><span class="default">$needle</span><span class="keyword">));<br></span><span class="comment">/* Output:<br>&#xA0;&#xA0; array(3) {<br>&#xA0; &#xA0;&#xA0; [&quot;fox&quot;]=&gt;<br>&#xA0; &#xA0;&#xA0; int(16)<br>&#xA0; &#xA0;&#xA0; [&quot;dog&quot;]=&gt;<br>&#xA0; &#xA0;&#xA0; int(40)<br>&#xA0; &#xA0;&#xA0; [&quot;.&quot;]=&gt;<br>&#xA0; &#xA0;&#xA0; int(43)<br>&#xA0; &#xA0;&#xA0; [&quot;duck&quot;]=&gt;<br>&#xA0; &#xA0;&#xA0; bool(false)<br>&#xA0;&#xA0; }<br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stripos.php)

**[To root](/README.md)**