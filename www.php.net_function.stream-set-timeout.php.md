# stream_set_timeout




<div class="phpcode"><span class="html">
In case anyone is puzzled, stream_set_timeout DOES NOT work for sockets created with socket_create or socket_accept. Use socket_set_option instead.<br><br>Instead of:<br><span class="default">&lt;?php<br>stream_set_timeout</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">,</span><span class="default">$sec</span><span class="keyword">,</span><span class="default">$usec</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Use:<br><span class="default">&lt;?php<br>socket_set_option</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">, </span><span class="default">SOL_SOCKET</span><span class="keyword">, </span><span class="default">SO_RCVTIMEO</span><span class="keyword">, array(</span><span class="string">&apos;sec&apos;</span><span class="keyword">=&gt;</span><span class="default">$sec</span><span class="keyword">, </span><span class="string">&apos;usec&apos;</span><span class="keyword">=&gt;</span><span class="default">$usec</span><span class="keyword">));<br></span><span class="default">socket_set_option</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">, </span><span class="default">SOL_SOCKET</span><span class="keyword">, </span><span class="default">SO_SNDTIMEO</span><span class="keyword">, array(</span><span class="string">&apos;sec&apos;</span><span class="keyword">=&gt;</span><span class="default">$sec</span><span class="keyword">, </span><span class="string">&apos;usec&apos;</span><span class="keyword">=&gt;</span><span class="default">$usec</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-set-timeout.php)

**[⬆ to root](/)**