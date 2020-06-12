# HTTP authentication with PHP




<div class="phpcode"><span class="html">
Workaround for missing Authorization header under CGI/FastCGI Apache:<br><br>SetEnvIf Authorization .+ HTTP_AUTHORIZATION=$0<br><br>Now PHP should automatically declare $_SERVER[PHP_AUTH_*] variables if the client sends the Authorization header.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is the simplest form I found to do a Basic authorization with retries.<br><br><span class="default">&lt;?php<br><br>$valid_passwords </span><span class="keyword">= array (</span><span class="string">&quot;mario&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;carbonell&quot;</span><span class="keyword">);<br></span><span class="default">$valid_users </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$valid_passwords</span><span class="keyword">);<br><br></span><span class="default">$user </span><span class="keyword">= </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;PHP_AUTH_USER&apos;</span><span class="keyword">];<br></span><span class="default">$pass </span><span class="keyword">= </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;PHP_AUTH_PW&apos;</span><span class="keyword">];<br><br></span><span class="default">$validated </span><span class="keyword">= (</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$user</span><span class="keyword">, </span><span class="default">$valid_users</span><span class="keyword">)) &amp;&amp; (</span><span class="default">$pass </span><span class="keyword">== </span><span class="default">$valid_passwords</span><span class="keyword">[</span><span class="default">$user</span><span class="keyword">]);<br><br>if (!</span><span class="default">$validated</span><span class="keyword">) {<br>&#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;WWW-Authenticate: Basic realm=&quot;My Realm&quot;&apos;</span><span class="keyword">);<br>&#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;HTTP/1.0 401 Unauthorized&apos;</span><span class="keyword">);<br>&#xA0; die (</span><span class="string">&quot;Not authorized&quot;</span><span class="keyword">);<br>}<br><br></span><span class="comment">// If arrives here, is a valid user.<br></span><span class="keyword">echo </span><span class="string">&quot;&lt;p&gt;Welcome </span><span class="default">$user</span><span class="string">.&lt;/p&gt;&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;&lt;p&gt;Congratulation, you are into the system.&lt;/p&gt;&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.http-auth.php)

**[To root](/)**