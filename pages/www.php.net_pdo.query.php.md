# PDO::query





The handling of errors by this function is controlled by the attribute PDO::ATTR_ERRMODE.

Use the following to make it throw an exception:


```
<?php
$dbh-&gt;setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdo.query.php)

**[To root](/README.md)**