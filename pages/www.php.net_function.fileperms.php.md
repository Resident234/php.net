# fileperms




<div class="phpcode"><span class="html">
Don&apos;t use substr, use bit operator<br><span class="default">&lt;?php<br>decoct</span><span class="keyword">(</span><span class="default">fileperms</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">) &amp; </span><span class="default">0777</span><span class="keyword">); </span><span class="comment">// return &quot;755&quot; for example<br></span><span class="default">?&gt;<br></span><br>If you want to compare permission<br><span class="default">&lt;?php<br>0755 </span><span class="keyword">=== (</span><span class="default">fileperms</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">) &amp; </span><span class="default">0777</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fileperms.php)

**[To root](/README.md)**