# Installation



Do not lose your time to install it on Ubuntu just trying "sudo apt-get install php5-memcached". There is something you need to do that sure installing memcached. Anyway...<br><br>Step 1.<br>$ sudo apt-get install memcached<br>Step 2.<br>$ sudo apt-get install php5-memcached<br>Step 3.<br>$ sudo /etc/init.d/apache2 restart<br><br>Ready!<br><br>What about some test?<br><br>

```
<?php
error_reporting(E_ALL &amp; ~E_NOTICE);

$mc = new Memcached();
$mc->addServer("localhost", 11211);

$mc->set("foo", "Hello!");
$mc->set("bar", "Memcached...");

$arr = array(
    $mc->get("foo"),
    $mc->get("bar")
);
var_dump($arr);
?>
```
<br><br>Hoping to help someone.<br>~Kerem  

---

[Official documentation page](https://www.php.net/manual/en/memcached.installation.php)

**[To root](/README.md)**