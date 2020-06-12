# imap_body




<div class="phpcode"><span class="html">
Simple example on how to read body message of the recent mail.
<br>
<br><span class="default">&lt;?php
<br>$imap </span><span class="keyword">= </span><span class="default">imap_open</span><span class="keyword">(</span><span class="string">&quot;{pop.example.com:995/pop3/ssl/novalidate-cert}&quot;</span><span class="keyword">, </span><span class="string">&quot;username&quot;</span><span class="keyword">, </span><span class="string">&quot;password&quot;</span><span class="keyword">);
<br>
<br>if( </span><span class="default">$imap </span><span class="keyword">) {
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">//Check no.of.msgs
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$num </span><span class="keyword">= </span><span class="default">imap_num_msg</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">//if there is a message in your inbox
<br>&#xA0; &#xA0;&#xA0; </span><span class="keyword">if( </span><span class="default">$num </span><span class="keyword">&gt;</span><span class="default">0 </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//read that mail recently arrived
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">imap_qprint</span><span class="keyword">(</span><span class="default">imap_body</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">));
<br>&#xA0; &#xA0;&#xA0; }
<br>
<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">//close the stream
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">imap_close</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-body.php)

**[⬆ to root](/)**