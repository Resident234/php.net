# Sessions support




<div class="phpcode"><span class="html">
If you want to use &apos;memcacheD&apos; extention not &apos;memcache&apos; (there are two diffrent extentions) for session control,&#xA0; you should pay attention to modify php.ini
<br>
<br>Most web resource from google is based on memcache because It&apos;s earlier version than memcacheD. They will say as following
<br>
<br>session.save_handler = memcache
<br>session.save_path = &quot;tcp://localhost:11211&quot;
<br>
<br>But it&apos;s not valid when it comes to memcacheD
<br>
<br>you should modify php.ini like that
<br>
<br>session.save_handler = memcached
<br>session.save_path = &quot;localhost:11211&quot;
<br>
<br>Look, there is no protocol indentifier</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are setting data to the session and it immediately disappears and you aren&apos;t getting any warnings in your PHP error log, it&apos;s probably because your sessions expired sometime in the 1970s.<br><br>Somewhere between memcached 1.0.2 and 2.1.0, the memcached session handler became sensitive to the 30-day TTL gotcha (aka &quot;transparent failover&quot;).&#xA0; If your session.gc_maxlifetime is greater than 2592000 (30 days), the value is treated as a unix timestamp instead of a relative seconds count.<br><br>This issue is likely to impact anyone with long-running sessions who is upgrading from Ubuntu 12.04 to 14.04.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The documentation is not complete, you can also pass the weight of each server and you can use sockets if you want. In your PHP ini:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// Sockets with weight in the format socket_path:port:weight<br></span><span class="default">session</span><span class="keyword">.</span><span class="default">save_path </span><span class="keyword">= </span><span class="string">&quot;/path/to/socket:0:42&quot;<br><br></span><span class="comment">// Or more than one so that weight makes sense?<br></span><span class="default">session</span><span class="keyword">.</span><span class="default">save_path </span><span class="keyword">= </span><span class="string">&quot;/path/to/socket_x:0:42,/path/to/socket_y:0:666&quot;<br><br></span><span class="default">?&gt;<br></span><br>And if you should ever want to access these servers in PHP:<br><br><span class="default">&lt;?php<br><br>$servers </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;,&quot;</span><span class="keyword">, </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&quot;session.save_path&quot;</span><span class="keyword">));<br></span><span class="default">$c </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$servers</span><span class="keyword">);<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$c</span><span class="keyword">; ++</span><span class="default">$i</span><span class="keyword">) {<br>&#xA0; </span><span class="default">$servers</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;:&quot;</span><span class="keyword">, </span><span class="default">$servers</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);<br>}<br></span><span class="default">$memcached </span><span class="keyword">= new \</span><span class="default">Memcached</span><span class="keyword">();<br></span><span class="default">call_user_func_array</span><span class="keyword">([ </span><span class="default">$memcached</span><span class="keyword">, </span><span class="string">&quot;addServers&quot; </span><span class="keyword">], </span><span class="default">$servers</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$memcached</span><span class="keyword">-&gt;</span><span class="default">getAllKeys</span><span class="keyword">());<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are using the memcache class for session handling your key is the PHP session ID.&#xA0; This is different than when using the&#xA0; memcached class.<br><br>Example with memcache:<br>GET nphu2u8eo5niltfgdbc33ajb62<br><br>Example with memcached:<br>GET memc.sess.key.nphu2u8eo5niltfgdbc33ajb62<br><br>For memcached, the prefix is set in the config:<br>memcached.sess_prefix = &quot;memc.sess.key.&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.sessions.php)

**[To root](/README.md)**