# ip2long




<div class="phpcode"><span class="html">
A quick method to convert a netmask (ex: 255.255.255.240) to a cidr mask (ex: /28):<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">mask2cidr</span><span class="keyword">(</span><span class="default">$mask</span><span class="keyword">){<br>&#xA0; </span><span class="default">$long </span><span class="keyword">= </span><span class="default">ip2long</span><span class="keyword">(</span><span class="default">$mask</span><span class="keyword">);<br>&#xA0; </span><span class="default">$base </span><span class="keyword">= </span><span class="default">ip2long</span><span class="keyword">(</span><span class="string">&apos;255.255.255.255&apos;</span><span class="keyword">);<br>&#xA0; return </span><span class="default">32</span><span class="keyword">-</span><span class="default">log</span><span class="keyword">((</span><span class="default">$long </span><span class="keyword">^ </span><span class="default">$base</span><span class="keyword">)+</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">);<br><br>&#xA0; </span><span class="comment">/* xor-ing will give you the inverse mask,<br>&#xA0; &#xA0; &#xA0; log base 2 of that +1 will return the number<br>&#xA0; &#xA0; &#xA0; of bits that are off in the mask and subtracting<br>&#xA0; &#xA0; &#xA0; from 32 gets you the cidr notation */<br>&#xA0; &#xA0; &#xA0; &#xA0; <br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">/*<br>&#xA0; &#xA0;&#xA0; Given an IP address and Subnet Mask, <br>&#xA0; &#xA0;&#xA0; determine the subnet address, broadcast address, and wildcard mask<br>&#xA0; &#xA0;&#xA0; by using bitwise operators<br><br>&#xA0; &#xA0;&#xA0; ref:&#xA0; <a href="http://php.net/manual/en/language.operators.bitwise.php" rel="nofollow" target="_blank">http://php.net/manual/en/language.operators.bitwise.php</a><br>*/<br><br></span><span class="default">$ip</span><span class="keyword">=</span><span class="string">&apos;10.10.10.7&apos;</span><span class="keyword">;<br></span><span class="default">$mask</span><span class="keyword">=</span><span class="string">&apos;255.255.255.0&apos;</span><span class="keyword">;<br></span><span class="default">$wcmask</span><span class="keyword">=</span><span class="default">long2ip</span><span class="keyword">( ~</span><span class="default">ip2long</span><span class="keyword">(</span><span class="default">$mask</span><span class="keyword">) );<br></span><span class="default">$subnet</span><span class="keyword">=</span><span class="default">long2ip</span><span class="keyword">( </span><span class="default">ip2long</span><span class="keyword">(</span><span class="default">$ip</span><span class="keyword">) &amp; </span><span class="default">ip2long</span><span class="keyword">(</span><span class="default">$mask</span><span class="keyword">) );<br></span><span class="default">$bcast</span><span class="keyword">=</span><span class="default">long2ip</span><span class="keyword">( </span><span class="default">ip2long</span><span class="keyword">(</span><span class="default">$ip</span><span class="keyword">) | </span><span class="default">ip2long</span><span class="keyword">(</span><span class="default">$wcmask</span><span class="keyword">) );<br>echo </span><span class="string">&quot;Given address </span><span class="default">$ip</span><span class="string"> and mask </span><span class="default">$mask</span><span class="string">, \n&quot; </span><span class="keyword">. <br></span><span class="string">&quot;the subnet is </span><span class="default">$subnet</span><span class="string"> and broadcast is </span><span class="default">$bcast</span><span class="string"> \n&quot; </span><span class="keyword">. <br></span><span class="string">&quot;with a wildcard mask of </span><span class="default">$wcmask</span><span class="string">&quot;</span><span class="keyword">;<br><br></span><span class="comment">/*<br>Given address 10.10.10.7 and mask 255.255.255.0, <br>the subnet is 10.10.10.0 and broadcast is 10.10.10.255 <br>with a wildcard mask of 0.0.0.255<br>*/<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To nate, who advises that there is no reason to use an unsigned version of the IP in a MySQL DB:<br><br>I think it would depend on your application, but personally, I find it useful to store IP&apos;s as unsigneds since MySQL has 2 native functions, INET_ATON() and INET_NTOA(), which work the same as ip2long()/long2ip() _except_ that they generate the unsigned counterpart. So if you want, you could do:<br><br>-- IANA Class-B reserved/private<br>SELECT * FROM `servers` <br>WHERE `ip` &gt;= INET_ATON(&apos;192.168.0.0&apos;) <br>AND `ip` &lt;= INET_ATON(&apos;192.168.255.255&apos;);<br><br>In my current application, I find it easier to use the MySQL built-ins than the PHP counter-parts. <br><br>In case you&apos;re curious as to the names ATON and NTOA:<br><br>ATON = address to number aka. ip2long<br>NTOA = number to address aka. long2ip</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ip2long.php)

**[⬆ to root](/)**