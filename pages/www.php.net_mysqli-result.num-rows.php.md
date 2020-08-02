# mysqli_result::$num_rows



If you have problems making work this num_rows, you have to declare -&gt;store_result() first.<br><br>

```
<?php
$mysqli = new mysqli("localhost","root", "", "tables");

$query = $mysqli->prepare("SELECT * FROM table1");
$query->execute();
$query->store_result();

$rows = $query->num_rows;

echo $rows;

// Return 4 for example
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-result.num-rows.php)

**[To root](/README.md)**