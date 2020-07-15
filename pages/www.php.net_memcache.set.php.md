# Memcache::set



This is just two minor things about memcache that might not be perfectly clear, the limits on key and data sizes and what happen to flags in the memcache protocol.<br><br>* There is a max key size of 250 anything bigger gets truncated. There is also a (1MB - 42 bytes) limit on the data.<br><br>* In the memcache protocol there is a 16bit, 32bit in newer version, flag that you can set to whatever you want because memcache doesn&apos;t do anything with the flags. The php api doesn&apos;t let you get the flags because php uses the flags for php&apos;s own use such as "MEMCACHE_COMPRESSED" and I decided to test if it was doing something because it wasn&apos;t part of the memcache protocol.<br><br>

```
<?php
$memcache = new Memcache();
$memcache->connect("127.0.0.1", 11211);

// Since memcache truncates the keys at 250 bytes both the get "250 a's" and "251 a's" will find the key in the cache
echo "*** Truncate key test ***&lt;br&gt;";
echo "set 251: " . ($memcache->set(str_repeat("a", 251), "value", 0, 1) ? "t" : "f") . "&lt;br&gt;";

echo "get 249: " . (($ret = $memcache->get(str_repeat("a", 249))) !== false ? "'$ret'" : "f") . "&lt;br&gt;";
echo "get 250: " . (($ret = $memcache->get(str_repeat("a", 250))) !== false ? "'$ret'" : "f") . "&lt;br&gt;";
echo "get 251: " . (($ret = $memcache->get(str_repeat("a", 251))) !== false ? "'$ret'" : "f") . "&lt;br&gt;";
echo "delete: " . ($memcache->delete(str_repeat("a", 250)) ? "t" : "f") . "&lt;br&gt;&lt;br&gt;";

echo "*** Compress value test ***&lt;br&gt;";
echo "set 1024*1024-42: " . ($memcache->set("test", str_repeat("a", 1024*1024-42), 0, 1) ? "t" : "f") . "&lt;br&gt;";
echo "set 1024*1024-41: " . ($memcache->set("test", str_repeat("a", 1024*1024-41), 0, 1) ? "t" : "f") . "&lt;br&gt;";
echo "set 1024*1024 compressed: " . ($memcache->set("test", str_repeat("a", 1024*1024), MEMCACHE_COMPRESSED, 1) ? "t" : "f") . "&lt;br&gt;";
echo "delete: " . ($memcache->delete("test") ? "t" : "f") . "&lt;br&gt;";
$memcache->close();
?>
```
<br><br>Output:<br>*** Truncate key test ***<br>set 251: t<br>get 249: f<br>get 250: &apos;value&apos;<br>get 251: &apos;value&apos;<br>delete: t<br><br>*** Compress value test ***<br>set 1024*1024-42: t<br>set 1024*1024-41: f<br>set 1024*1024 compressed: t<br>delete: t  

#

[Official documentation page](https://www.php.net/manual/en/memcache.set.php)

**[To root](/README.md)**