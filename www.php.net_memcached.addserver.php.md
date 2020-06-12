# Memcached::addServer




<div class="phpcode"><span class="html">
Important to not call -&gt;addServers() every run -- only call it if no servers exist (check getServerList() ); otherwise, since addServers() does not check for dups, it will let you add the same server again and again and again, resultings in hundreds if not thousands of connections to the MC daemon. Specially when using FastCGI.
<br>
<br>Example:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">Cache </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$id</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$obj</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">id </span><span class="keyword">= </span><span class="default">$id</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">obj </span><span class="keyword">= new </span><span class="default">Memcached</span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">connect</span><span class="keyword">(</span><span class="default">$host </span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$servers </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">obj</span><span class="keyword">-&gt;</span><span class="default">getServerList</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$servers</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$servers </span><span class="keyword">as </span><span class="default">$server</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$server</span><span class="keyword">[</span><span class="string">&apos;host&apos;</span><span class="keyword">] == </span><span class="default">$host </span><span class="keyword">and </span><span class="default">$server</span><span class="keyword">[</span><span class="string">&apos;port&apos;</span><span class="keyword">] == </span><span class="default">$port</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">obj</span><span class="keyword">-&gt;</span><span class="default">addServer</span><span class="keyword">(</span><span class="default">$host </span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As of version 2.0.0b1 you can use Unix socket.<br><br><span class="default">&lt;?php<br>$m </span><span class="keyword">= new </span><span class="default">Memcached</span><span class="keyword">();<br></span><span class="default">$m</span><span class="keyword">-&gt;</span><span class="default">addServer</span><span class="keyword">(</span><span class="string">&apos;/path/to/socket&apos;</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Not to be confused with Memcache that use &apos;unix:///path/to/socket&apos;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.addserver.php)

**[⬆ to root](/)**