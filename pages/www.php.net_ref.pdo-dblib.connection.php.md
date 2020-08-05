# PDO_DBLIB DSN



Instead of specifying tds version and client charset in freetds.conf, you may pass it as a parameter.<br>

```
<?php $dsn = 'dblib:version=7.0;charset=UTF-8;host=domain.example.com;dbname=example;'; ?>
```
  

---

If you&apos;re using FreeTDS driver and you want to use "charset" parameter then you may have to edit freetds.conf (e.g. /etc/freetds/freetds.conf) and force connection using at least version 7.0 of the protocol.<br><br>tds version = 7.0<br><br>Charset parameter accepts all encodings supported by iconv (execute iconv --list to show all encodings).  

---

[Official documentation page](https://www.php.net/manual/en/ref.pdo-dblib.connection.php)

**[To root](/README.md)**