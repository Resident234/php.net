# SQLite3Stmt::bindValue





Note that this also works with positional placeholders using the &apos;?&apos; token:



```
<?php

$stmt = $db-&gt;prepare(&apos;SELECT * FROM mytable WHERE foo = ? AND bar = ?&apos;);
$stmt-&gt;bindValue(1, &apos;somestring&apos;, SQLITE3_TEXT);
$stmt-&gt;bindValue(2, 42, SQLITE3_INTEGER);

?>
```


Positional numbering starts at 1.

  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3stmt.bindvalue.php)

**[To root](/README.md)**