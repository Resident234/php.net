# imap_open




<div class="phpcode"><span class="html">
Using: 
<br><span class="default">&lt;?php
<br>imap_open</span><span class="keyword">( </span><span class="string">&quot;{server.example.com:143}INBOX&quot; </span><span class="keyword">, </span><span class="string">&apos;login&apos; </span><span class="keyword">, </span><span class="string">&apos;password&apos; </span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Got this error:
<br>&quot;Couldn&apos;t open stream {server.example.com:143}INBOX&quot; 
<br>
<br>Solved by adding the flag &quot;novalidate-cert&quot;:
<br><span class="default">&lt;?php
<br>imap_open</span><span class="keyword">( </span><span class="string">&quot;{server.example.com:143/novalidate-cert}INBOX&quot; </span><span class="keyword">, </span><span class="string">&apos;login&apos; </span><span class="keyword">, </span><span class="string">&apos;password&apos; </span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>=D</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-open.php)

**[⬆ to root](/)**