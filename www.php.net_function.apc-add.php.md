# apc_add




<div class="phpcode"><span class="html">
In order to understand better how APC caching works you can do the following:<br><br>1. Restart web server (Apache or Nginx)<br><br>2. Create file &quot;apc_fetch.php&quot;:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">apc_fetch</span><span class="keyword">(array(<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_5s_1&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_5s_2&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_5s_3&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_0s_1&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_0s_2&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_0s_3&apos;</span><span class="keyword">,<br>)));<br></span><span class="default">?&gt;<br></span><br>3. Create file &quot;apc_add.php&quot;:<br><span class="default">&lt;?php<br>$ttl </span><span class="keyword">= </span><span class="default">5</span><span class="keyword">;<br><br></span><span class="default">$key </span><span class="keyword">= </span><span class="string">&apos;CUR_DATE_5s_1&apos;</span><span class="keyword">;<br></span><span class="default">$var </span><span class="keyword">= </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">apc_add</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">, </span><span class="default">$ttl</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br>echo </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">$var </span><span class="keyword">= </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">apc_add</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">, </span><span class="default">$ttl</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br>echo </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">$key </span><span class="keyword">= </span><span class="string">&apos;CUR_DATE_0s_1&apos;</span><span class="keyword">;<br></span><span class="default">$var </span><span class="keyword">= </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">apc_add</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br>echo </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">$values </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_5s_2&apos; </span><span class="keyword">=&gt; </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_5s_3&apos; </span><span class="keyword">=&gt; </span><span class="default">rand</span><span class="keyword">(),<br>);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">apc_add</span><span class="keyword">(</span><span class="default">$values</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">, </span><span class="default">$ttl</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br>echo </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">$values </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_0s_2&apos; </span><span class="keyword">=&gt; </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;c&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="string">&apos;CUR_DATE_0s_3&apos; </span><span class="keyword">=&gt; </span><span class="default">rand</span><span class="keyword">(),<br>);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">apc_add</span><span class="keyword">(</span><span class="default">$values</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>4. Run &quot;apc_fetch.php&quot; and &quot;apc_add.php&quot; several times in order to see the persistent result and how values change from one request to another.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.apc-add.php)

**[To root](/README.md)**