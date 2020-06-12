# Socket Functions




<div class="phpcode"><span class="html">
After many non-sleep nights I got the most simple multi-client server written in PHP that really works. Ctrl+C and Ctrl+V... use as command line to test it. Enjoy it.
<br>
<br><span class="default">&lt;?php
<br>
<br>ini_set</span><span class="keyword">(</span><span class="string">&apos;error_reporting&apos;</span><span class="keyword">, </span><span class="default">E_ALL </span><span class="keyword">^ </span><span class="default">E_NOTICE</span><span class="keyword">);
<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;display_errors&apos;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br>
<br></span><span class="comment">// Set time limit to indefinite execution
<br></span><span class="default">set_time_limit </span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br>
<br></span><span class="comment">// Set the ip and port we will listen on
<br></span><span class="default">$address </span><span class="keyword">= </span><span class="string">&apos;10.203.9.67&apos;</span><span class="keyword">;
<br></span><span class="default">$port </span><span class="keyword">= </span><span class="default">6901</span><span class="keyword">;
<br>
<br></span><span class="comment">// Create a TCP Stream socket
<br></span><span class="default">$sock </span><span class="keyword">= </span><span class="default">socket_create</span><span class="keyword">(</span><span class="default">AF_INET</span><span class="keyword">, </span><span class="default">SOCK_STREAM</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);
<br>
<br></span><span class="comment">// Bind the socket to an address/port
<br></span><span class="default">socket_bind</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">, </span><span class="default">$address</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">) or die(</span><span class="string">&apos;Could not bind to address&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// Start listening for connections
<br></span><span class="default">socket_listen</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">);
<br>
<br></span><span class="comment">// Non block socket type
<br></span><span class="default">socket_set_nonblock</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">);
<br>
<br></span><span class="comment">// Loop continuously
<br></span><span class="keyword">while (</span><span class="default">true</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; unset(</span><span class="default">$read</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$j </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$client</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$client </span><span class="keyword">AS </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$read</span><span class="keyword">[</span><span class="default">$j</span><span class="keyword">] = </span><span class="default">$v</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$j</span><span class="keyword">++;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">$client </span><span class="keyword">= </span><span class="default">$read</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$newsock </span><span class="keyword">= @</span><span class="default">socket_accept</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_resource</span><span class="keyword">(</span><span class="default">$newsock</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">socket_write</span><span class="keyword">(</span><span class="default">$newsock</span><span class="keyword">, </span><span class="string">&quot;</span><span class="default">$j</span><span class="string">&gt;&quot;</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">).</span><span class="default">chr</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;New client connected </span><span class="default">$j</span><span class="string">&quot;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$client</span><span class="keyword">[</span><span class="default">$j</span><span class="keyword">] = </span><span class="default">$newsock</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$j</span><span class="keyword">++;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$client</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$client </span><span class="keyword">AS </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (@</span><span class="default">socket_recv</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">, </span><span class="default">MSG_DONTWAIT</span><span class="keyword">) === </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$client</span><span class="keyword">[</span><span class="default">$k</span><span class="keyword">]);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">socket_close</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$string</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$k</span><span class="string">: </span><span class="default">$string</span><span class="string">\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="comment">//echo &quot;.&quot;;
<br>
<br>&#xA0; &#xA0; </span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);
<br>}
<br>
<br></span><span class="comment">// Close the master sockets
<br></span><span class="default">socket_close</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.sockets.php)

**[⬆ to root](/)**