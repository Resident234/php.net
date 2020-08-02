# PDOStatement::rowCount



When updating a Mysql table with identical values nothing&apos;s really affected so rowCount will return 0. As Mr. Perl below noted this is not always preferred behaviour and you can change it yourself since PHP 5.3.<br><br>Just create your PDO object with <br>

```
<?php
$p = new PDO($dsn, $u, $p, array(PDO::MYSQL_ATTR_FOUND_ROWS => true));
?>
```
<br>and rowCount() will tell you how many rows your update-query actually found/matched.  

---

Great, while using MySQL5, the only way to get the number of rows after doing a PDO SELECT query is to either execute a separate SELECT COUNT(*) query (or to do count($stmt-&gt;fetchAll()), which seems like a ridiculous waste of overhead and programming time.<br><br>Another gripe I have about PDO is its inability to get the value of output parameters from stored procedures in some DBMSs, such as SQL Server.<br><br>I&apos;m not so sure I&apos;m diggin&apos; PDO yet.  

---

Note that an INSERT ... ON DUPLICATE KEY UPDATE statement is not an INSERT statement, rowCount won&apos;t return the number or rows inserted or updated for such a statement.  For MySQL, it will return 1 if the row is inserted, and 2 if it is updated, but that may not apply to other databases.  

---

To display information only when the query is not empty, I do something like this:<br><br>

```
<?php
    $sql = 'SELECT model FROM cars';
    $stmt = $db->prepare($sql);
    $stmt->execute();
    
    if ($data = $stmt->fetch()) {
        do {
            echo $data['model'] . '<br>';
        } while ($data = $stmt->fetch());
    } else {
        echo 'Empty Query';
    }
?>
```
  

---

It&apos;d better to use SQL_CALC_FOUND_ROWS, if you only use MySQL. It has many advantages as you could retrieve only part of result set (via LIMIT) but still get the total row count.<br>code:<br>

```
<?php
$db = new PDO(DSN...);
$db->setAttribute(array(PDO::MYSQL_USE_BUFFERED_QUERY=>TRUE));
$rs  = $db->query('SELECT SQL_CALC_FOUND_ROWS * FROM table LIMIT 5,15');
$rs1 = $db->query('SELECT FOUND_ROWS()');
$rowCount = (int) $rs1->fetchColumn();
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/pdostatement.rowcount.php)

**[To root](/README.md)**