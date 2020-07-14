# PDOStatement::errorCode



Statement&apos;s errorCode() returns an empty string before execution, and &apos;00000&apos; (five zeros) after a sucessfull execution:<br><br>

```
<?php
$stmt = $pdo-&gt;prepare($query);
assert($stmt-&gt;errorCode === &apos;&apos;);

$stmt-&gt;execute();
assert($stmt-&gt;errorCode === &apos;00000&apos;);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.errorcode.php)

**[To root](/README.md)**