# PDOStatement::fetchColumn



fetchColumn return boolean false when a row not is found or don&apos;t had more rows.  

---

This is an excellent method for returning a column count. For example:<br><br>

```
<?php
$db = new PDO('mysql:host=localhost;dbname=pictures','user','password');
$pics = $db->query('SELECT COUNT(id) FROM pics');
$this->totalpics = $pics->fetchColumn();
$db = null;
?>
```
<br>In my case $pics-&gt;fetchColumn() returns 641 because that is how many pictures I have in my db.  

---

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchcolumn.php)

**[To root](/README.md)**