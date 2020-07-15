# mysqli::$affected_rows



On "INSERT INTO ON DUPLICATE KEY UPDATE" queries, though one may expect affected_rows to return only 0 or 1 per row on successful queries, it may in fact return 2.<br><br>From Mysql manual: "With ON DUPLICATE KEY UPDATE, the affected-rows value per row is 1 if the row is inserted as a new row and 2 if an existing row is updated."<br><br>See: http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html<br><br>Here&apos;s the sum breakdown _per row_:<br>+0: a row wasn&apos;t updated or inserted (likely because the row already existed, but no field values were actually changed during the UPDATE)<br>+1: a row was inserted<br>+2: a row was updated  

#

If you need to know specifically whether the WHERE condition of an UPDATE operation failed to match rows, or that simply no rows required updating you need to instead check mysqli::$info.<br><br>As this returns a string that requires parsing, you can use the following to convert the results into an associative array.<br><br>Object oriented style:<br><br>

```
<?php
    preg_match_all ('/(\S[^:]+): (\d+)/', $mysqli->info, $matches); 
    $info = array_combine ($matches[1], $matches[2]);
?>
```


Procedural style:



```
<?php
    preg_match_all ('/(\S[^:]+): (\d+)/', mysqli_info ($link), $matches); 
    $info = array_combine ($matches[1], $matches[2]);
?>
```


You can then use the array to test for the different conditions



```
<?php
    if ($info ['Rows matched'] == 0) {
        echo "This operation did not match any rows.\n";
    } elseif ($info ['Changed'] == 0) {
        echo "This operation matched rows, but none required updating.\n";
    }

    if ($info ['Changed'] < $info ['Rows matched']) {
        echo ($info ['Rows matched'] - $info ['Changed'])." rows matched but were not changed.\n";
    }
?>
```
<br><br>This approach can be used with any query that mysqli::$info supports (INSERT INTO, LOAD DATA, ALTER TABLE, and UPDATE), for other any queries it returns an empty array.<br><br>For any UPDATE operation the array returned will have the following elements:<br><br>Array<br>(<br>    [Rows matched] =&gt; 1<br>    [Changed] =&gt; 0<br>    [Warnings] =&gt; 0<br>)  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.affected-rows.php)

**[To root](/README.md)**