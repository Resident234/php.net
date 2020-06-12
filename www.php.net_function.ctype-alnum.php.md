# ctype_alnum




<div class="phpcode"><span class="html">
ctype_alnum() is a godsend for quick and easy username/data filtering when used in conjunction with str_replace().
<br>
<br>Let&apos;s say your usernames have dash(-) and underscore(_) allowable and alphanumeric digits as well.
<br>
<br>Instead of a regex you can trade a bit of performance for simplicity:
<br>
<br><span class="default">&lt;?php
<br>$sUser </span><span class="keyword">= </span><span class="string">&apos;my_username01&apos;</span><span class="keyword">;
<br></span><span class="default">$aValid </span><span class="keyword">= array(</span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="string">&apos;_&apos;</span><span class="keyword">);
<br>
<br>if(!</span><span class="default">ctype_alnum</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$aValid</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$sUser</span><span class="keyword">))) {
<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Your username is not properly formatted.&apos;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Quicktip: If ctype is not enabled by default on your server, replace ctype_alnum($var) with preg_match(&apos;/^[a-zA-Z0-9]+$/&apos;, $var).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ctype-alnum.php)

**[⬆ to root](/)**