# odbc_prepare



Is it just me or is the code above misleading? It makes it look like odbc_execute() returns a resource suitable, say, for passing to one of the odbc_fetch_* functions.<br><br>In fact, odbc_execute() returns a boolean, which simply indicates success (TRUE) or failure (FALSE). The variable to pass to odbc_fetch_* is the same one that you pass to odbc_execute():<br><br>

```
<?php
$res = odbc_prepare($db_conn, $query_string);
if(!$res) die("could not prepare statement ".$query_string);

if(odbc_execute($res, $parameters)) {
    $row = odbc_fetch_array($res);
} else {
    // handle error
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.odbc-prepare.php)

**[To root](/README.md)**