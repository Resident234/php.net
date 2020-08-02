# Memcached::__construct



When using persistent connections, it is important to not re-add servers.<br><br>This is what you do not want to do:<br>

```
<?php
$mc = new Memcached('mc');
$mc->setOption(Memcached::OPT_LIBKETAMA_COMPATIBLE, true);
$mc->addServers(array(
    array('mc1.example.com',11211),
    array('mc2.example.com',11211),
));
?>
```

Every time the page is loaded those servers will be appended to the list resulting in many simultaneous open connections to the same server. The addServer/addServers functions to not check for existing references to the specified servers.

A better approach is something like:


```
<?php
$mc = new Memcached('mc');
$mc->setOption(Memcached::OPT_LIBKETAMA_COMPATIBLE, true);
if (!count($mc->getServerList())) {
    $mc->addServers(array(
        array('mc1.example.com',11211),
        array('mc2.example.com',11211),
    ));
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/memcached.construct.php)

**[To root](/README.md)**