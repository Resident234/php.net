# Memcache::set




<div class="phpcode"><span class="html">
This is just two minor things about memcache that might not be perfectly clear, the limits on key and data sizes and what happen to flags in the memcache protocol.
<br>
<br>* There is a max key size of 250 anything bigger gets truncated. There is also a (1MB - 42 bytes) limit on the data.
<br>
<br>* In the memcache protocol there is a 16bit, 32bit in newer version, flag that you can set to whatever you want because memcache doesn&apos;t do anything with the flags. The php api doesn&apos;t let you get the flags because php uses the flags for php&apos;s own use such as &quot;MEMCACHE_COMPRESSED&quot; and I decided to test if it was doing something because it wasn&apos;t part of the memcache protocol.
<br>
<br><span class="default">&lt;?php
<br>$memcache </span><span class="keyword">= new </span><span class="default">Memcache</span><span class="keyword">();
<br></span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">connect</span><span class="keyword">(</span><span class="string">&quot;127.0.0.1&quot;</span><span class="keyword">, </span><span class="default">11211</span><span class="keyword">);
<br>
<br></span><span class="comment">// Since memcache truncates the keys at 250 bytes both the get &quot;250 a&apos;s&quot; and &quot;251 a&apos;s&quot; will find the key in the cache
<br></span><span class="keyword">echo </span><span class="string">&quot;*** Truncate key test ***&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;set 251: &quot; </span><span class="keyword">. (</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">251</span><span class="keyword">), </span><span class="string">&quot;value&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">) ? </span><span class="string">&quot;t&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>
<br>echo </span><span class="string">&quot;get 249: &quot; </span><span class="keyword">. ((</span><span class="default">$ret </span><span class="keyword">= </span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">249</span><span class="keyword">))) !== </span><span class="default">false </span><span class="keyword">? </span><span class="string">&quot;&apos;</span><span class="default">$ret</span><span class="string">&apos;&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;get 250: &quot; </span><span class="keyword">. ((</span><span class="default">$ret </span><span class="keyword">= </span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">250</span><span class="keyword">))) !== </span><span class="default">false </span><span class="keyword">? </span><span class="string">&quot;&apos;</span><span class="default">$ret</span><span class="string">&apos;&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;get 251: &quot; </span><span class="keyword">. ((</span><span class="default">$ret </span><span class="keyword">= </span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">251</span><span class="keyword">))) !== </span><span class="default">false </span><span class="keyword">? </span><span class="string">&quot;&apos;</span><span class="default">$ret</span><span class="string">&apos;&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;delete: &quot; </span><span class="keyword">. (</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">delete</span><span class="keyword">(</span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">250</span><span class="keyword">)) ? </span><span class="string">&quot;t&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&lt;br&gt;&quot;</span><span class="keyword">;
<br>
<br>echo </span><span class="string">&quot;*** Compress value test ***&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;set 1024*1024-42: &quot; </span><span class="keyword">. (</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="string">&quot;test&quot;</span><span class="keyword">, </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">-</span><span class="default">42</span><span class="keyword">), </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">) ? </span><span class="string">&quot;t&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;set 1024*1024-41: &quot; </span><span class="keyword">. (</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="string">&quot;test&quot;</span><span class="keyword">, </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">-</span><span class="default">41</span><span class="keyword">), </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">) ? </span><span class="string">&quot;t&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;set 1024*1024 compressed: &quot; </span><span class="keyword">. (</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="string">&quot;test&quot;</span><span class="keyword">, </span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">), </span><span class="default">MEMCACHE_COMPRESSED</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">) ? </span><span class="string">&quot;t&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br>echo </span><span class="string">&quot;delete: &quot; </span><span class="keyword">. (</span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">delete</span><span class="keyword">(</span><span class="string">&quot;test&quot;</span><span class="keyword">) ? </span><span class="string">&quot;t&quot; </span><span class="keyword">: </span><span class="string">&quot;f&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br></span><span class="default">$memcache</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>Output:
<br>*** Truncate key test ***
<br>set 251: t
<br>get 249: f
<br>get 250: &apos;value&apos;
<br>get 251: &apos;value&apos;
<br>delete: t
<br>
<br>*** Compress value test ***
<br>set 1024*1024-42: t
<br>set 1024*1024-41: f
<br>set 1024*1024 compressed: t
<br>delete: t</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcache.set.php)

**[⬆ to root](/)**