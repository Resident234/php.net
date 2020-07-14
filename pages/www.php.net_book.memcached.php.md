# Memcached



For those confuse about the memcached extension and the memcache extension, the short story is that both of them are clients of memcached server, and the memcached extension offer more features than the memcache extension.  

#

GOTCHA: Recently I was tasked with moving from PECL memcache to PECL memcached and ran into a major problem -- memcache and memcached serialize data differently, meaning that data written with one library can&apos;t necessarily be read with the other library.<br><br>For example, If you write an object or an array with memcache, it&apos;s interpreted as an integer by memcached.  If you write it with memcached, it&apos;s interpreted as a string by memcache.<br><br>tl;dr - You can&apos;t safely switch between memcache and memcached without a either a cache flush or isolated cache environments.<br><br>

```
<?php
$memcache = new Memcache;
$memcacheD = new Memcached;
$memcache->addServer($host);
$memcacheD->addServers($servers);

$checks = array(
    123,
    4542.32,
    'a string',
    true,
    array(123, 'string'),
    (object)array('key1' => 'value1'),
);
foreach ($checks as $i => $value) {
    print "Checking WRITE with Memcache\n";
    $key = 'cachetest' . $i;
    $memcache->set($key, $value);
    usleep(100);
    $val = $memcache->get($key);
    $valD = $memcacheD->get($key);
    if ($val !== $valD) {
        print "Not compatible!";
        var_dump(compact('val', 'valD'));
    }

    print "Checking WRITE with MemcacheD\n";
    $key = 'cachetest' . $i;
    $memcacheD->set($key, $value);
    usleep(100);
    $val = $memcache->get($key);
    $valD = $memcacheD->get($key);
    if ($val !== $valD) {
        print "Not compatible!";
        var_dump(compact('val', 'valD'));
    }
}?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/book.memcached.php)

**[To root](/README.md)**