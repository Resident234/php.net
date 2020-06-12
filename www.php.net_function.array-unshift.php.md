# array_unshift




<div class="phpcode"><span class="html">
You can preserve keys and unshift an array with numerical indexes in a really simple way if you&apos;ll do the following:
<br>
<br><span class="default">&lt;?php
<br>$someArray</span><span class="keyword">=array(</span><span class="default">224</span><span class="keyword">=&gt;</span><span class="string">&apos;someword1&apos;</span><span class="keyword">, </span><span class="default">228</span><span class="keyword">=&gt;</span><span class="string">&apos;someword2&apos;</span><span class="keyword">, </span><span class="default">102</span><span class="keyword">=&gt;</span><span class="string">&apos;someword3&apos;</span><span class="keyword">, </span><span class="default">544</span><span class="keyword">=&gt;</span><span class="string">&apos;someword3&apos;</span><span class="keyword">,</span><span class="default">95</span><span class="keyword">=&gt;</span><span class="string">&apos;someword4&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">$someArray</span><span class="keyword">=array(</span><span class="default">100</span><span class="keyword">=&gt;</span><span class="string">&apos;Test Element 1 &apos;</span><span class="keyword">,</span><span class="default">255</span><span class="keyword">=&gt;</span><span class="string">&apos;Test Element 2&apos;</span><span class="keyword">)+</span><span class="default">$someArray</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>now the array looks as follows:
<br>
<br>array(
<br>100=&gt;&apos;Test Element 1 &apos;,
<br>255=&gt;&apos;Test Element 2&apos;
<br>224=&gt;&apos;someword1&apos;,
<br>228=&gt;&apos;someword2&apos;,
<br>102=&gt;&apos;someword3&apos;,
<br>544=&gt;&apos;someword3&apos;,
<br>95=&gt;&apos;someword4&apos;
<br>);</span>
</div>
  

#


<div class="phpcode"><span class="html">
array_merge() will also reindex (see array_merge() manual entry), but the &apos;+&apos; operator won&apos;t, so...
<br>
<br><span class="default">&lt;?php
<br>$arrayone</span><span class="keyword">=array(</span><span class="string">&quot;newkey&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;newvalue&quot;</span><span class="keyword">) + </span><span class="default">$arrayone</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>does the job.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sahn&apos;s example almost works but has a small error. Try it like this if you need to prepend something to the array without the keys being reindexed and/or need to prepend a key value pair, you can use this short function: <br><br><span class="default">&lt;?php <br></span><span class="keyword">function </span><span class="default">array_unshift_assoc</span><span class="keyword">(&amp;</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">, </span><span class="default">$val</span><span class="keyword">) <br>{ <br>&#xA0; &#xA0; </span><span class="default">$arr </span><span class="keyword">= </span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); <br>&#xA0; &#xA0; </span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">; <br>&#xA0; &#xA0; return = </span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">); <br>} <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Anonymous&apos; associative version wasn&apos;t working for me, but it did with this small tweak:<br><br>function array_unshift_assoc(&amp;$arr, $key, $val) <br>{ <br>&#xA0; &#xA0; $arr = array_reverse($arr, true); <br>&#xA0; &#xA0; $arr[$key] = $val; <br>&#xA0; &#xA0; $arr = array_reverse($arr, true); <br>&#xA0; &#xA0; return $arr;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-unshift.php)

**[⬆ to root](/)**