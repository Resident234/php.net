# PDOStatement::setFetchMode



if you want to fetch your result into a class (by using PDO::FETCH_CLASS) and want the constructor to be executed *before* PDO assings the object properties, you need to use the PDO::FETCH_PROPS_LATE constant:<br><br>

```
<?php
$stmt = $pdo->prepare("your query");

$stmt->setFetchMode(PDO::FETCH_CLASS|PDO::FETCH_PROPS_LATE, "className", $constructorArguments);

# pass parameters, if required by the query
$stmt->execute($parameters);

foreach ($stmt as $row)
{
    // do something with (each of) your object
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/pdostatement.setfetchmode.php)

**[To root](/README.md)**