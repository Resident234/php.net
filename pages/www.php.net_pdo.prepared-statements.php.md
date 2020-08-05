# Prepared statements and stored procedures



Note that when using name parameters with bindParam, the name itself, cannot contain a dash &apos;-&apos;. <br><br>example:<br>

```
<?php
$stmt = $dbh->prepare ("INSERT INTO user (firstname, surname) VALUES (:f-name, :s-name)");
$stmt -> bindParam(':f-name', 'John');
$stmt -> bindParam(':s-name', 'Smith');
$stmt -> execute();
?>
```
<br><br>The dashes in &apos;f-name&apos; and &apos;s-name&apos; should be replaced with an underscore or no dash at all.<br><br>See http://bugs.php.net/43130<br><br>Adam  

---

[Official documentation page](https://www.php.net/manual/en/pdo.prepared-statements.php)

**[To root](/README.md)**