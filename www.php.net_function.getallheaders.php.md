# getallheaders




<div class="phpcode"><span class="html">
it could be useful if you using nginx instead of apache
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;getallheaders&apos;</span><span class="keyword">)) 
<br>{
<br>&#xA0; &#xA0; function </span><span class="default">getallheaders</span><span class="keyword">() 
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$headers </span><span class="keyword">= [];
<br>&#xA0; &#xA0; &#xA0;&#xA0; foreach (</span><span class="default">$_SERVER </span><span class="keyword">as </span><span class="default">$name </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) 
<br>&#xA0; &#xA0; &#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">) == </span><span class="string">&apos;HTTP_&apos;</span><span class="keyword">) 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$headers</span><span class="keyword">[</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="default">ucwords</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;_&apos;</span><span class="keyword">, </span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">)))))] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">$headers</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There&apos;s a polyfill for this that can be downloaded or installed via composer:<br><br><a href="https://github.com/ralouphie/getallheaders" rel="nofollow" target="_blank">https://github.com/ralouphie/getallheaders</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware that RFC2616 (HTTP/1.1) defines header fields as case-insensitive entities. Therefore, array keys of getallheaders() should be converted first to lower- or uppercase and processed such.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.getallheaders.php)

**[⬆ to root](/)**