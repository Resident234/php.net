# The mysqli_result class



Converting an old project from using the mysql extension to the mysqli extension, I found the most annoying change to be the lack of a corresponding mysql_result function in mysqli. While mysql_result is a generally terrible function, it was useful for fetching a single result field *value* from a result set (for example, if looking up a user&apos;s ID).<br><br>The behavior of mysql_result is approximated here, though you may want to name it something other than mysqli_result so as to avoid thinking it&apos;s an actual, built-in function.<br><br>

```
<?php
function mysqli_result($res, $row, $field=0) {
    $res->data_seek($row);
    $datarow = $res->fetch_array();
    return $datarow[$field];
}
?>
```
<br><br>Implementing it via the OO interface is left as an exercise to the reader.  

---

An "mysqli_result" function where $field can be like table_name.field_name with alias or not.<br>

```
<?php
function mysqli_result($result,$row,$field=0) {
    if ($result===false) return false;
    if ($row>=mysqli_num_rows($result)) return false;
    if (is_string($field) &amp;&amp; !(strpos($field,".")===false)) {
        $t_field=explode(".",$field);
        $field=-1;
        $t_fields=mysqli_fetch_fields($result);
        for ($id=0;$id<mysqli_num_fields($result);$id++) {
            if ($t_fields[$id]->table==$t_field[0] &amp;&amp; $t_fields[$id]->name==$t_field[1]) {
                $field=$id;
                break;
            }
        }
        if ($field==-1) return false;
    }
    mysqli_data_seek($result,$row);
    $line=mysqli_fetch_array($result);
    return isset($line[$field])?$line[$field]:false;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.mysqli-result.php)

**[To root](/README.md)**