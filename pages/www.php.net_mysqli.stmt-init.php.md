# mysqli::stmt_init



stmt_init() seems to clear previous (possibly erroneous) results on the DB connection, which means you don&apos;t necessarily need to use it but it could make the code more robust.<br><br>In a PHPUnit test, I had a sequence of prepared queries on the same connection. One of them fetched a row from a SELECT but didn&apos;t keep fetching until it drained the connection, so it left some stale results. When the next query did this:<br><br>

```
<?php
$db = $this->getConnection()->getDbConnection();
$preparedQuery = $db->prepare ($query);
?>
```


the prepare() call generated an error: "Could not prepare query: Commands out of sync; you can't run this command now." Changing to this:



```
<?php
$db = $this->getConnection()->getDbConnection();
$preparedQuery = $db->stmt_init();
$preparedQuery->prepare ($query);
?>
```
<br><br>resolved the problem.  

#

you can use $stmt = $mysqli-&gt;prepare(); directly without stmt-init() . i think there is no need for stmt-init .  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.stmt-init.php)

**[To root](/README.md)**