# Memcache::get



$flags stays untouched if $key was not found on the server, it&apos;s helpfull to determine if bool(false) was stored:<br><br>

```
<?php

$memcache = new Memcache();

$memcache->set('test', false); // 
$flags = false;
var_dump($memcache->get('test', $flags)); // bool(false)
var_dump($flags); // int(256) - changed to int

$memcache->delete('test');

$flags = false;
var_dump($memcache->get('test', $flags)); // bool(false)
var_dump($flags); // bool(false) - untouched

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/memcache.get.php)

**[To root](/README.md)**