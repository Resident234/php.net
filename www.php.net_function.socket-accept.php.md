# socket_accept




<div class="phpcode"><span class="html">
If you want to have multiple clients on a server you will have to use non blocking.<br><br><span class="default">&lt;?php<br><br>$clients </span><span class="keyword">= array();<br></span><span class="default">$socket </span><span class="keyword">= </span><span class="default">socket_create</span><span class="keyword">(</span><span class="default">AF_INET</span><span class="keyword">,</span><span class="default">SOCK_STREAM</span><span class="keyword">,</span><span class="default">SOL_TCP</span><span class="keyword">);<br></span><span class="default">socket_bind</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">,</span><span class="string">&apos;127.0.0.1&apos;</span><span class="keyword">,</span><span class="default">$port</span><span class="keyword">);<br></span><span class="default">socket_listen</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">);<br></span><span class="default">socket_set_nonblock</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">);<br><br>while(</span><span class="default">true</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if((</span><span class="default">$newc </span><span class="keyword">= </span><span class="default">socket_accept</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">)) !== </span><span class="default">false</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Client </span><span class="default">$newc</span><span class="string"> has connected\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$clients</span><span class="keyword">[] = </span><span class="default">$newc</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to make a daemon process that forks on each client connection, you&apos;ll find out that there&apos;s a bug in PHP. The child processes send SIGCHLD to their parent when they exit but the parent can&apos;t handle signals while it&apos;s waiting for socket_accept (blocking).
<br>
<br>Here is a piece of code that makes a non-blocking forking server.
<br>
<br>#!/usr/bin/php -q
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br>&#xA0; * Listens for requests and forks on each connection
<br>&#xA0; */
<br>
<br></span><span class="default">$__server_listening </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>
<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);
<br></span><span class="default">set_time_limit</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br></span><span class="default">ob_implicit_flush</span><span class="keyword">();
<br>declare(</span><span class="default">ticks </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">);
<br>
<br></span><span class="default">become_daemon</span><span class="keyword">();
<br>
<br></span><span class="comment">/* nobody/nogroup, change to your host&apos;s uid/gid of the non-priv user */
<br></span><span class="default">change_identity</span><span class="keyword">(</span><span class="default">65534</span><span class="keyword">, </span><span class="default">65534</span><span class="keyword">);
<br>
<br></span><span class="comment">/* handle signals */
<br></span><span class="default">pcntl_signal</span><span class="keyword">(</span><span class="default">SIGTERM</span><span class="keyword">, </span><span class="string">&apos;sig_handler&apos;</span><span class="keyword">);
<br></span><span class="default">pcntl_signal</span><span class="keyword">(</span><span class="default">SIGINT</span><span class="keyword">, </span><span class="string">&apos;sig_handler&apos;</span><span class="keyword">);
<br></span><span class="default">pcntl_signal</span><span class="keyword">(</span><span class="default">SIGCHLD</span><span class="keyword">, </span><span class="string">&apos;sig_handler&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">/* change this to your own host / port */
<br></span><span class="default">server_loop</span><span class="keyword">(</span><span class="string">&quot;127.0.0.1&quot;</span><span class="keyword">, </span><span class="default">1234</span><span class="keyword">);
<br>
<br></span><span class="comment">/**
<br>&#xA0; * Change the identity to a non-priv user
<br>&#xA0; */
<br></span><span class="keyword">function </span><span class="default">change_identity</span><span class="keyword">( </span><span class="default">$uid</span><span class="keyword">, </span><span class="default">$gid </span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if( !</span><span class="default">posix_setgid</span><span class="keyword">( </span><span class="default">$gid </span><span class="keyword">) )
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;Unable to setgid to &quot; </span><span class="keyword">. </span><span class="default">$gid </span><span class="keyword">. </span><span class="string">&quot;!\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if( !</span><span class="default">posix_setuid</span><span class="keyword">( </span><span class="default">$uid </span><span class="keyword">) )
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;Unable to setuid to &quot; </span><span class="keyword">. </span><span class="default">$uid </span><span class="keyword">. </span><span class="string">&quot;!\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="comment">/**
<br>&#xA0; * Creates a server socket and listens for incoming client connections
<br>&#xA0; * @param string $address The address to listen on
<br>&#xA0; * @param int $port The port to listen on
<br>&#xA0; */
<br></span><span class="keyword">function </span><span class="default">server_loop</span><span class="keyword">(</span><span class="default">$address</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; GLOBAL </span><span class="default">$__server_listening</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if((</span><span class="default">$sock </span><span class="keyword">= </span><span class="default">socket_create</span><span class="keyword">(</span><span class="default">AF_INET</span><span class="keyword">, </span><span class="default">SOCK_STREAM</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">)) &lt; </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;failed to create socket: &quot;</span><span class="keyword">.</span><span class="default">socket_strerror</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit();
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if((</span><span class="default">$ret </span><span class="keyword">= </span><span class="default">socket_bind</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">, </span><span class="default">$address</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">)) &lt; </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;failed to bind socket: &quot;</span><span class="keyword">.</span><span class="default">socket_strerror</span><span class="keyword">(</span><span class="default">$ret</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit();
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if( ( </span><span class="default">$ret </span><span class="keyword">= </span><span class="default">socket_listen</span><span class="keyword">( </span><span class="default">$sock</span><span class="keyword">, </span><span class="default">0 </span><span class="keyword">) ) &lt; </span><span class="default">0 </span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;failed to listen to socket: &quot;</span><span class="keyword">.</span><span class="default">socket_strerror</span><span class="keyword">(</span><span class="default">$ret</span><span class="keyword">).</span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit();
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">socket_set_nonblock</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;waiting for clients to connect\n&quot;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; while (</span><span class="default">$__server_listening</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$connection </span><span class="keyword">= @</span><span class="default">socket_accept</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$connection </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">usleep</span><span class="keyword">(</span><span class="default">100</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }elseif (</span><span class="default">$connection </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">handle_client</span><span class="keyword">(</span><span class="default">$sock</span><span class="keyword">, </span><span class="default">$connection</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }else
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;error: &quot;</span><span class="keyword">.</span><span class="default">socket_strerror</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="comment">/**
<br>&#xA0; * Signal handler
<br>&#xA0; */
<br></span><span class="keyword">function </span><span class="default">sig_handler</span><span class="keyword">(</span><span class="default">$sig</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; switch(</span><span class="default">$sig</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">SIGTERM</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">SIGINT</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; exit();
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">SIGCHLD</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">pcntl_waitpid</span><span class="keyword">(-</span><span class="default">1</span><span class="keyword">, </span><span class="default">$status</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="comment">/** 
<br>&#xA0; * Handle a new client connection
<br>&#xA0; */
<br></span><span class="keyword">function </span><span class="default">handle_client</span><span class="keyword">(</span><span class="default">$ssock</span><span class="keyword">, </span><span class="default">$csock</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; GLOBAL </span><span class="default">$__server_listening</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; </span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$pid </span><span class="keyword">== -</span><span class="default">1</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* fork failed */
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo </span><span class="string">&quot;fork failure!\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; die;
<br>&#xA0; &#xA0; }elseif (</span><span class="default">$pid </span><span class="keyword">== </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* child process */
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$__server_listening </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">socket_close</span><span class="keyword">(</span><span class="default">$ssock</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">interact</span><span class="keyword">(</span><span class="default">$csock</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">socket_close</span><span class="keyword">(</span><span class="default">$csock</span><span class="keyword">);
<br>&#xA0; &#xA0; }else
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">socket_close</span><span class="keyword">(</span><span class="default">$csock</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>function </span><span class="default">interact</span><span class="keyword">(</span><span class="default">$socket</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="comment">/* TALK TO YOUR CLIENT */
<br></span><span class="keyword">}
<br>
<br></span><span class="comment">/**
<br>&#xA0; * Become a daemon by forking and closing the parent
<br>&#xA0; */
<br></span><span class="keyword">function </span><span class="default">become_daemon</span><span class="keyword">()
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$pid </span><span class="keyword">= </span><span class="default">pcntl_fork</span><span class="keyword">();
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if (</span><span class="default">$pid </span><span class="keyword">== -</span><span class="default">1</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* fork failed */
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo </span><span class="string">&quot;fork failure!\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit();
<br>&#xA0; &#xA0; }elseif (</span><span class="default">$pid</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* close the parent */
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">exit();
<br>&#xA0; &#xA0; }else
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/* child becomes our daemon */
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">posix_setsid</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">chdir</span><span class="keyword">(</span><span class="string">&apos;/&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">umask</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">posix_getpid</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-accept.php)

**[To root](/README.md)**