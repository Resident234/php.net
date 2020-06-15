# Output Control FunctionsSee Also




<div class="phpcode"><span class="html">
It seems that while using output buffering, an included file which calls die() before the output buffer is closed is flushed rather than cleaned. That is, ob_end_flush() is called by default.<br><br><span class="default">&lt;?php<br></span><span class="comment">// a.php (this file should never display anything)<br></span><span class="default">ob_start</span><span class="keyword">();<br>include(</span><span class="string">&apos;b.php&apos;</span><span class="keyword">);<br></span><span class="default">ob_end_clean</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="comment">// b.php<br></span><span class="keyword">print </span><span class="string">&quot;b&quot;</span><span class="keyword">;<br>die();<br></span><span class="default">?&gt;<br></span><br>This ends up printing &quot;b&quot; rather than nothing as ob_end_flush() is called instead of ob_end_clean(). That is, die() flushes the buffer rather than cleans it. This took me a while to determine what was causing the flush, so I thought I&apos;d share.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.outcontrol.php)

**[To root](/README.md)**