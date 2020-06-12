# mb_substr




<div class="phpcode"><span class="html">
Passing null as length will not make mb_substr use it&apos;s default, instead it will interpret it as 0.<br><span class="default">&lt;?php<br>mb_substr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">,</span><span class="default">$start</span><span class="keyword">,</span><span class="default">null</span><span class="keyword">,</span><span class="default">$encoding</span><span class="keyword">); </span><span class="comment">//Returns &apos;&apos; (empty string) just like substr()<br></span><span class="default">?&gt;<br></span>Instead use:<br><span class="default">&lt;?php<br>mb_substr</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">,</span><span class="default">$start</span><span class="keyword">,</span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">),</span><span class="default">$encoding</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-substr.php)

**[⬆ to root](/)**