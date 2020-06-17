# get_magic_quotes_gpc




<div class="phpcode"><span class="html">
@ dot dot dot dot dot alexander at gmail dot com
<br>
<br>I suggest replacing foreach by &quot;stripslashes_deep&quot;:
<br>
<br>Example #2 Using stripslashes() on an array on 
<br>&lt;<a href="http://www.php.net/manual/en/function.stripslashes.php&gt;:" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.stripslashes.php&gt;:</a>
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">stripslashes_deep</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">) ?
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;stripslashes_deep&apos;</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">) :
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">stripslashes</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$value</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>This gives:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if((</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&quot;get_magic_quotes_gpc&quot;</span><span class="keyword">) &amp;&amp; </span><span class="default">get_magic_quotes_gpc</span><span class="keyword">())&#xA0; &#xA0; || (</span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&apos;magic_quotes_sybase&apos;</span><span class="keyword">) &amp;&amp; (</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&apos;magic_quotes_sybase&apos;</span><span class="keyword">))!=</span><span class="string">&quot;off&quot;</span><span class="keyword">)) ){
<br>&#xA0; &#xA0; </span><span class="default">stripslashes_deep</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">stripslashes_deep</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">stripslashes_deep</span><span class="keyword">(</span><span class="default">$_COOKIE</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-magic-quotes-gpc.php)

**[To root](/README.md)**