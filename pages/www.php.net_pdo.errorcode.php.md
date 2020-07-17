# PDO::errorCode



Hi,<br><br>List containing all SQL-92 SQLSTATE Return Codes:<br>http://www.unix.org.ua/orelly/java-ent/jenut/ch08_06.htm<br><br>Use the following Code to let PDO throw Exceptions (PDOException) on Error.<br><br>

```
<?php 
$pdo = new PDO (whatever);
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
try {
    $pdo->exec ("QUERY WITH SYNTAX ERROR");
} catch (PDOException $e) {
    if ($e->getCode() == '2A000')
        echo "Syntax Error: ".$e->getMessage();
}
?>
```
<br><br>Bye,<br>  Matthias  

#

[Official documentation page](https://www.php.net/manual/en/pdo.errorcode.php)

**[To root](/README.md)**