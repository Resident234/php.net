# SQLite3::query





The recommended way to do a SQLite3 query is to use a statement. For a table creation, a query might be fine (and easier) but for an insert, update or select, you should really use a statement, it&apos;s really easier and safer as SQLite will escape your parameters according to their type. SQLite will also use less memory than if you created the whole query by yourself. Example:



```
<?php

$db = new SQLite3;
$statement = $db-&gt;prepare(&apos;SELECT * FROM table WHERE id = :id;&apos;);
$statement-&gt;bindValue(&apos;:id&apos;, $id);

$result = $statement-&gt;execute();

?>
```


You can also re-use a statement and change its parameters, just do $statement-&gt;reset(). Finally don&apos;t forget to close a statement when you don&apos;t need it anymore as it will free some memory.

  

#

[Official documentation page](https://www.php.net/manual/en/sqlite3.query.php)

**[To root](/README.md)**