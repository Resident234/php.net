# imap_utf8




<div class="phpcode"><span class="html">
That fixed the all caps issue:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">imap_utf8_fix</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {
<br>&#xA0; return </span><span class="default">iconv_mime_decode</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-utf8.php)

**[To root](/README.md)**