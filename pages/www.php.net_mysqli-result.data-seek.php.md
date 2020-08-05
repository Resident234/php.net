# mysqli_result::data_seek



This is useful function when you try to loop through the resultset numerous times.<br><br>For example:<br><br>

```
<?php

$result = mysqli_query($connection_id,$query);

while ($row = mysqli_fetch_assoc($result))
 {
  // Looping through the resultset.
 }

// Now if you need to loop through it again, you would first call the seek function:
mysqli_data_seek($result,0);

while ($row = mysqli_fetch_assoc($result))
 {
  // Looping through the resultset again.
 }

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-result.data-seek.php)

**[To root](/README.md)**