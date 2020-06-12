# func_num_args




<div class="phpcode"><span class="html">
Just a note for anyone wondering. This function doesn&apos;t include params that have a default value, unless you pass one in to overwrite the default param value. Not sure if that makes sense, so here&apos;s an example:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">helloWorld</span><span class="keyword">(</span><span class="default">$ArgA</span><span class="keyword">, </span><span class="default">$ArgB</span><span class="keyword">=</span><span class="string">&quot;HelloWorld!&quot;</span><span class="keyword">) {
<br>&#xA0; return </span><span class="default">func_num_args</span><span class="keyword">();
<br>}
<br>
<br></span><span class="comment">// The following will return 1
<br></span><span class="default">$Returns1 </span><span class="keyword">= </span><span class="default">helloWorld</span><span class="keyword">(</span><span class="string">&quot;HelloWorld!&quot;</span><span class="keyword">);
<br>
<br></span><span class="comment">// The following will return 2
<br></span><span class="default">$Returns2 </span><span class="keyword">= </span><span class="default">helloWorld</span><span class="keyword">(</span><span class="string">&quot;HelloWorld!&quot;</span><span class="keyword">, </span><span class="string">&quot;HowdyWorld!&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.func-num-args.php)

**[⬆ to root](/)**