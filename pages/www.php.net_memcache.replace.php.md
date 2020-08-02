# Memcache::replace



This page mentions that replace should be used rather than set, but gives no reason. Best information I could find was a comment by &apos;argyleblanket&apos; on the set page. (http://www.php.net/manual/en/memcache.set.php#84032)<br><br>"Using set more than once for the same key seems to have unexpected results - it does not behave as a "replace," but instead seems to "set" more than one value for the same key.  "get" may return any of the values. <br><br>This was tested on a multiple-server setup - behaviour may be different if you only have one server. "  

---

[Official documentation page](https://www.php.net/manual/en/memcache.replace.php)

**[To root](/README.md)**