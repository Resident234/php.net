# Memcached::set



In the example it shows:<br><br>/* expire &apos;object&apos; key in 5 minutes */<br>$m-&gt;set(&apos;object&apos;, new stdclass, time() + 300);<br><br>But this is wrong.<br>It will not expire, at least, not for a long long time.<br>So instead of time() + seconds, you use:<br>$m-&gt;set(&apos;object&apos;, new stdclass, 300);<br>And it will correctly expire after 5 minutes.<br><br>Note: Using memcached 2.1.0 stable through PECL.  

---

[Official documentation page](https://www.php.net/manual/en/memcached.set.php)

**[To root](/README.md)**