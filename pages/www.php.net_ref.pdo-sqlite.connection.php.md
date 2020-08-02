# PDO_SQLITE DSN



In memory sqlite has some limitations. The memory space could be the request, the session, but no way seems documented to share a base in memory among users.<br><br>For a request, open your base with the code<br>$pdo = new PDO( &apos;sqlite::memory:&apos;);<br>and your base will disapear on next request.<br><br>For session persistency<br>

```
<?php
$pdo = new PDO(
    'sqlite::memory:',
    null,
    null,
    array(PDO::ATTR_PERSISTENT => true)
);
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/ref.pdo-sqlite.connection.php)

**[To root](/README.md)**