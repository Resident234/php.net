# Memcached::getVersion



Was going mad, I figured getStatus() would return false on servers not responding. This is however not true, it will return:<br>array(1) {<br>  ["127.0.0.1:11112"]=&gt;<br>  string(11) "255.255.255"<br>}<br>On a failed connection.  

---

[Official documentation page](https://www.php.net/manual/en/memcached.getversion.php)

**[To root](/README.md)**