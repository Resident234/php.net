# Installation




<div class="phpcode"><span class="html">
Do not lose your time to install it on Ubuntu just trying &quot;sudo apt-get install php5-memcached&quot;. There is something you need to do that sure installing memcached. Anyway...
<br>
<br>Step 1.
<br>$ sudo apt-get install memcached
<br>Step 2.
<br>$ sudo apt-get install php5-memcached
<br>Step 3.
<br>$ sudo /etc/init.d/apache2 restart
<br>
<br>Ready!
<br>
<br>What about some test?
<br>
<br><span class="default">&lt;?php
<br>error_reporting</span><span class="keyword">(</span><span class="default">E_ALL </span><span class="keyword">&amp; ~</span><span class="default">E_NOTICE</span><span class="keyword">);
<br>
<br></span><span class="default">$mc </span><span class="keyword">= new </span><span class="default">Memcached</span><span class="keyword">();
<br></span><span class="default">$mc</span><span class="keyword">-&gt;</span><span class="default">addServer</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">, </span><span class="default">11211</span><span class="keyword">);
<br>
<br></span><span class="default">$mc</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="string">&quot;foo&quot;</span><span class="keyword">, </span><span class="string">&quot;Hello!&quot;</span><span class="keyword">);
<br></span><span class="default">$mc</span><span class="keyword">-&gt;</span><span class="default">set</span><span class="keyword">(</span><span class="string">&quot;bar&quot;</span><span class="keyword">, </span><span class="string">&quot;Memcached...&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">$arr </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="default">$mc</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="string">&quot;foo&quot;</span><span class="keyword">),
<br>&#xA0; &#xA0; </span><span class="default">$mc</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="string">&quot;bar&quot;</span><span class="keyword">)
<br>);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Hoping to help someone.
<br>~Kerem</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.installation.php)

**[To root](/README.md)**