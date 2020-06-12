# stream_socket_client




<div class="phpcode"><span class="html">
For those wanting to use stream_socket_client() to connect to a local UNIX socket who can&apos;t find documentation on how to do it, here&apos;s a (rough) example:<br><br><span class="default">&lt;?php<br><br>$sock </span><span class="keyword">= </span><span class="default">stream_socket_client</span><span class="keyword">(</span><span class="string">&apos;unix:///full/path/to/my/socket.sock&apos;</span><span class="keyword">, </span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">);<br><br></span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">, </span><span class="string">&apos;SOME COMMAND&apos;</span><span class="keyword">.</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">);<br><br>echo </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">, </span><span class="default">4096</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stream-socket-client.php)

**[⬆ to root](/)**