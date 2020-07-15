# PDOStatement::errorCode



Statement&apos;s errorCode() returns an empty string before execution, and &apos;00000&apos; (five zeros) after a sucessfull execution:<br><br>

```
<?php
$stmt = $pdo->prepare($query);
assert($stmt->errorCode === '');

$stmt->execute();
assert($stmt->errorCode === '00000');
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.errorcode.php)

**[To root](/README.md)**