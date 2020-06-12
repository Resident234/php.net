# Output Buffering Control




<div class="phpcode"><span class="html">
The manual is a little shy on explaining that output buffers are nested, and that &quot;turns off output buffering&quot; means turning off the highest nested buffer.&#xA0; See ob_get_level (for a useful function, but still no explanation)
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; ob_start</span><span class="keyword">();
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;1:blah\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">ob_start</span><span class="keyword">();
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;2:blah&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// ob_get_clean() returns the contents of the last buffer opened.&#xA0; The first &quot;blah&quot; and the output of var_dump are flushed from the top buffer on exit
<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">ob_get_clean</span><span class="keyword">());
<br>&#xA0; &#xA0; exit;
<br></span><span class="default">?&gt;
<br></span>
<br>puts:
<br>&#xA0; &#xA0; 1:blah
<br>&#xA0; &#xA0; string(6) &quot;2:blah&quot;
<br>
<br>Prior to realising this, I had thought PHP&apos;s ob functionality left more to be desired.&#xA0; I *really* wish I knew earlier.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.outcontrol.php)

**[⬆ to root](/)**