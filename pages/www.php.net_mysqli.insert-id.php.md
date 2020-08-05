# mysqli::$insert_id



I have received many statements that the insert_id property has a bug because it "works sometimes".  Keep in mind that when using the OOP approach, the actual instantiation of the mysqli class will hold the insert_id.  <br><br>The following code will return nothing.<br>

```
<?php
$mysqli = new mysqli('host','user','pass','db');
if ($result = $mysqli->query("INSERT INTO t (field) VALUES ('value');")) {
   echo 'The ID is: '.$result->insert_id;
}
?>
```


This is because the insert_id property doesn't belong to the result, but rather the actual mysqli class.  This would work:



```
<?php
$mysqli = new mysqli('host','user','pass','db');
if ($result = $mysqli->query("INSERT INTO t (field) VALUES ('value');")) {
   echo 'The ID is: '.$mysqli->insert_id;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mysqli.insert-id.php)

**[To root](/README.md)**