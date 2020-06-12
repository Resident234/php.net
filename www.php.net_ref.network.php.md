# Network Functions




<div class="phpcode"><span class="html">
A simple and very fast function to check against CIDR.<br><br>Your previous examples are too complicated and involves a lot of functions call.<br><br>Here it is (only with arithmetic operators and call only to ip2long () and split() ):<br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">function </span><span class="default">ipCIDRCheck </span><span class="keyword">(</span><span class="default">$IP</span><span class="keyword">, </span><span class="default">$CIDR</span><span class="keyword">) {<br>&#xA0; &#xA0; list (</span><span class="default">$net</span><span class="keyword">, </span><span class="default">$mask</span><span class="keyword">) = </span><span class="default">split </span><span class="keyword">(</span><span class="string">&quot;/&quot;</span><span class="keyword">, </span><span class="default">$CIDR</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$ip_net </span><span class="keyword">= </span><span class="default">ip2long </span><span class="keyword">(</span><span class="default">$net</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$ip_mask </span><span class="keyword">= ~((</span><span class="default">1 </span><span class="keyword">&lt;&lt; (</span><span class="default">32 </span><span class="keyword">- </span><span class="default">$mask</span><span class="keyword">)) - </span><span class="default">1</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$ip_ip </span><span class="keyword">= </span><span class="default">ip2long </span><span class="keyword">(</span><span class="default">$IP</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$ip_ip_net </span><span class="keyword">= </span><span class="default">$ip_ip </span><span class="keyword">&amp; </span><span class="default">$ip_mask</span><span class="keyword">;<br><br>&#xA0; &#xA0; return (</span><span class="default">$ip_ip_net </span><span class="keyword">== </span><span class="default">$ip_net</span><span class="keyword">);<br>&#xA0; }<br></span><span class="default">?&gt;<br></span>call example: <span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">ipCheck </span><span class="keyword">(</span><span class="string">&quot;192.168.1.23&quot;</span><span class="keyword">, </span><span class="string">&quot;192.168.1.0/24&quot;</span><span class="keyword">); </span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.network.php)

**[⬆ to root](/)**