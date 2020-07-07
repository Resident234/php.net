# OpenSSL changes in PHP 5.6.x




<div class="phpcode"><span class="html">
To get back to the &quot;old behavior&quot; with self-signed (&quot;snakeoil&quot;) certificates to possibly incorrect name, you need to set both options.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $streamContext </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">([<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;ssl&apos; </span><span class="keyword">=&gt; [<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;verify_peer&apos;&#xA0; &#xA0; &#xA0; </span><span class="keyword">=&gt; </span><span class="default">false</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;verify_peer_name&apos; </span><span class="keyword">=&gt; </span><span class="default">false<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">]<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$contents </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;<a href="https://url" rel="nofollow" target="_blank">https://url</a>&apos;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">$streamContext</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration56.openssl.php)

**[To root](/README.md)**