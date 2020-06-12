# Internet Domain: TCP, UDP, SSL, and TLS




<div class="phpcode"><span class="html">
@pablo dot livardo&#xA0; :&#xA0; I think that the problem you found is caused by the difference between the client/server encryption methods used.<br><br>The 465 port is used for SMTPS, and the server starts the encryption immediately it receives your connection. So, your code will work.<br><br>The 587 port is used for Submission (MSA or Mail Submission Agent) which works like the port 25. The server accepts your connection and doesn&apos;t activate the encryption. If you want an encrypted connection on the port 587, you must connect on it without encryption, you must start to dialog with the server (with EHLO) and after that you must ask the server to start the encrypted connection using the STARTTLS command. The server starts the encryption and now you can start as well the encryption on your client. <br><br>So, in few words, you can not use : <br><br><span class="default">&lt;?php $fp </span><span class="keyword">= </span><span class="default">fsockopen</span><span class="keyword">(</span><span class="string">&quot;tls://mail.example.com&quot;</span><span class="keyword">, </span><span class="default">587</span><span class="keyword">, </span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">);&#xA0; </span><span class="default">?&gt;</span>&#xA0; <br><br>but you can use:<br><br> <span class="default">&lt;?php $fp </span><span class="keyword">= </span><span class="default">stream_socket_client</span><span class="keyword">(</span><span class="string">&quot;mail.example.com:587&quot;</span><span class="keyword">, </span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">); </span><span class="default">?&gt;</span>&#xA0; <br><br>and after you send the STARTTLS command, you can enable the crypto:<br><br><span class="default">&lt;?php stream_socket_enable_crypto</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">STREAM_CRYPTO_METHOD_SSLv23_CLIENT</span><span class="keyword">); </span><span class="default">?&gt;<br></span><br>P.S. My previous note on this page was totally wrong, so I ask the php.net admin to remove it.<br><br>:)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/transports.inet.php)

**[⬆ to root](/)**