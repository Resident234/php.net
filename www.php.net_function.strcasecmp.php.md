# strcasecmp




<div class="phpcode"><span class="html">
A simple multibyte-safe case-insensitive string comparison:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">mb_strcasecmp</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">, </span><span class="default">$encoding </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">null </span><span class="keyword">=== </span><span class="default">$encoding</span><span class="keyword">) { </span><span class="default">$encoding </span><span class="keyword">= </span><span class="default">mb_internal_encoding</span><span class="keyword">(); }<br>&#xA0; &#xA0; return </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">, </span><span class="default">$encoding</span><span class="keyword">), </span><span class="default">mb_strtoupper</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">, </span><span class="default">$encoding</span><span class="keyword">));<br>}<br><br></span><span class="default">?&gt;<br></span><br>Caveat: watch out for edge cases like &quot;&#xDF;&quot;.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The sample above is only true on some platforms that only use a simple &apos;C&apos; locale, where individual bytes are considered as complete characters that are converted to lowercase before being differentiated.<br><br>Other locales (see LC_COLLATE and LC_ALL) use the difference of collation order of characters, where characters may be groups of bytes taken from the input strings, or simply return -1, 0, or 1 as the collation order is not simply defined by comparing individual characters but by more complex rules.<br><br>Don&apos;t base your code on a specific non null value returned by strcmp() or strcasecmp(): it is not portable. Just consider the sign of the result and be sure to use the correct locale!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strcasecmp.php)

**[⬆ to root](/)**