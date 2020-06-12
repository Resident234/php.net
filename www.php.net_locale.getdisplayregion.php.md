# Locale::getDisplayRegion




<div class="phpcode"><span class="html">
You don&apos;t have to have a fully-formed locale for the first parameter. This makes the function useful for getting the country name from any locale:<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">Locale</span><span class="keyword">::</span><span class="default">getDisplayRegion</span><span class="keyword">(</span><span class="string">&apos;-US&apos;</span><span class="keyword">, </span><span class="string">&apos;fr&apos;</span><span class="keyword">));<br><br></span><span class="comment">//Returns<br></span><span class="default">string </span><span class="string">&apos;&#xC9;tats-Unis&apos; </span><span class="keyword">(</span><span class="default">length</span><span class="keyword">=</span><span class="default">11</span><span class="keyword">)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/locale.getdisplayregion.php)

**[⬆ to root](/)**