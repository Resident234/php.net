# Memcached



For those confuse about the memcached extension and the memcache extension, the short story is that both of them are clients of memcached server, and the memcached extension offer more features than the memcache extension.  

#

GOTCHA: Recently I was tasked with moving from PECL memcache to PECL memcached and ran into a major problem -- memcache and memcached serialize data differently, meaning that data written with one library can&apos;t necessarily be read with the other library.<br><br>For example, If you write an object or an array with memcache, it&apos;s interpreted as an integer by memcached.  If you write it with memcached, it&apos;s interpreted as a string by memcache.<br><br>tl;dr - You can&apos;t safely switch between memcache and memcached without a either a cache flush or isolated cache environments.<br><br>

```
<?php<br>$memcache = new Memcache;<br>$memcacheD = new Memcached;<br>$memcache-&gt;addServer($host);<br>$memcacheD-&gt;addServers($servers);<br><br>$checks = array(<br>    123,<br>    4542.32,<br>    &apos;a string&apos;,<br>    true,<br>    array(123, &apos;string&apos;),<br>    (object)array(&apos;key1&apos; =&gt; &apos;value1&apos;),<br>);<br>foreach ($checks as $i =&gt; $value) {<br>    print "Checking WRITE with Memcache\n";<br>    $key = &apos;cachetest&apos; . $i;<br>    $memcache-&gt;set($key, $value);<br>    usleep(100);<br>    $val = $memcache-&gt;get($key);<br>    $valD = $memcacheD-&gt;get($key);<br>    if ($val !== $valD) {<br>        print "Not compatible!";<br>        var_dump(compact(&apos;val&apos;, &apos;valD&apos;));<br>    }<br><br>    print "Checking WRITE with MemcacheD\n";<br>    $key = &apos;cachetest&apos; . $i;<br>    $memcacheD-&gt;set($key, $value);<br>    usleep(100);<br>    $val = $memcache-&gt;get($key);<br>    $valD = $memcacheD-&gt;get($key);<br>    if ($val !== $valD) {<br>        print "Not compatible!";<br>        var_dump(compact(&apos;val&apos;, &apos;valD&apos;));<br>    }<br>}  

#

[Official documentation page](https://www.php.net/manual/en/book.memcached.php)

**[To root](/README.md)**