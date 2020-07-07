# odbc_prepare





Is it just me or is the code above misleading? It makes it look like odbc_execute() returns a resource suitable, say, for passing to one of the odbc_fetch_* functions.

In fact, odbc_execute() returns a boolean, which simply indicates success (TRUE) or failure (FALSE). The variable to pass to odbc_fetch_* is the same one that you pass to odbc_execute():



```
<?php
$res = odbc_prepare($db_conn, $query_string);
if(!$res) die(&quot;could not prepare statement &quot;.$query_string);

if(odbc_execute($res, $parameters)) {
&#xA0; &#xA0; $row = odbc_fetch_array($res);
} else {
&#xA0; &#xA0; // handle error
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.odbc-prepare.php)

**[To root](/README.md)**