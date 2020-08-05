# SQLite3Stmt::bindValue



Note that this also works with positional placeholders using the &apos;?&apos; token:<br><br>

```
<?php

$stmt = $db->prepare('SELECT * FROM mytable WHERE foo = ? AND bar = ?');
$stmt->bindValue(1, 'somestring', SQLITE3_TEXT);
$stmt->bindValue(2, 42, SQLITE3_INTEGER);

?>
```
<br><br>Positional numbering starts at 1.  

---

[Official documentation page](https://www.php.net/manual/en/sqlite3stmt.bindvalue.php)

**[To root](/README.md)**