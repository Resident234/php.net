# FTP




<div class="phpcode"><span class="html">
For those who dont want to deal with handling the connection once created, here is a simple class that allows you to call any ftp function as if it were an extended method.&#xA0; It automatically puts the ftp connection into the first argument slot (as all ftp functions require).
<br>
<br>This code is php 5.3+
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">ftp</span><span class="keyword">{
<br>&#xA0; &#xA0; public </span><span class="default">$conn</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">conn </span><span class="keyword">= </span><span class="default">ftp_connect</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">__call</span><span class="keyword">(</span><span class="default">$func</span><span class="keyword">,</span><span class="default">$a</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">strstr</span><span class="keyword">(</span><span class="default">$func</span><span class="keyword">,</span><span class="string">&apos;ftp_&apos;</span><span class="keyword">) !== </span><span class="default">false </span><span class="keyword">&amp;&amp; </span><span class="default">function_exists</span><span class="keyword">(</span><span class="default">$func</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_unshift</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">,</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">conn</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">call_user_func_array</span><span class="keyword">(</span><span class="default">$func</span><span class="keyword">,</span><span class="default">$a</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// replace with your own error handler.
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">die(</span><span class="string">&quot;</span><span class="default">$func</span><span class="string"> is not a valid FTP function&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="comment">// Example
<br></span><span class="default">$ftp </span><span class="keyword">= new </span><span class="default">ftp</span><span class="keyword">(</span><span class="string">&apos;ftp.example.com&apos;</span><span class="keyword">);
<br></span><span class="default">$ftp</span><span class="keyword">-&gt;</span><span class="default">ftp_login</span><span class="keyword">(</span><span class="string">&apos;username&apos;</span><span class="keyword">,</span><span class="string">&apos;password&apos;</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$ftp</span><span class="keyword">-&gt;</span><span class="default">ftp_nlist</span><span class="keyword">());
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.ftp.php)

**[To root](/README.md)**