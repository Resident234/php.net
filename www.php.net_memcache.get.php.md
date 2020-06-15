# Memcache::get




<div class="phpcode"><span class="html">
$flags stays untouched if $key was not found on the server, it&apos;s helpfull to determine if bool(false) was stored:<br><br><span class="default">&lt;?php<br><br>$memcache </span><span class="keyword">= new </span><span class="default">Memcache</span><span class="keyword">();<br><br></span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="string">&apos;test&apos;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">); </span><span class="comment">// <br></span><span class="default">$flags </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="string">&apos;test&apos;</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">)); </span><span class="comment">// bool(false)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$flags</span><span class="keyword">); </span><span class="comment">// int(256) - changed to int<br><br></span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">delete</span><span class="keyword">(</span><span class="string">&apos;test&apos;</span><span class="keyword">);<br><br></span><span class="default">$flags </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="string">&apos;test&apos;</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">)); </span><span class="comment">// bool(false)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$flags</span><span class="keyword">); </span><span class="comment">// bool(false) - untouched<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcache.get.php)

**[To root](/README.md)**