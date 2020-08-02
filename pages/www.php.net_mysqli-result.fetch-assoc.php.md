# mysqli_result::fetch_assoc



I often like to have my results sent elsewhere in the format of an array (although keep in mind that if you just plan on traversing through the array in another part of the script, this extra step is just a waste of time).<br><br>This is my one-liner for transforming a mysqli_result set into an array.<br>

```
<?php
$sql = new MySQLi($host, $username, $password, $database);

$result = $sql->query("SELECT * FROM `$table`;");
for ($set = array (); $row = $result->fetch_assoc(); $set[] = $row);
print_r($set);
?>
```


Outputs:
Array
(
    [0] => Array
        (
            [id] => 1
            [field2] => a
            [field3] => b
        ),
    [1] => Array
        (
            [id] => 2
            [field2] => c
            [field3] => d
        )
)

I use other variations to adapt to the situation, i.e. if I am selecting only one field:


```
<?php
$sql = new MySQLi($host, $username, $password, $database);
$result = $sql->query("SELECT `field2` FROM `$table`;");
for ($set = array (); $row = $result->fetch_assoc(); $set[] = $row['field2']);
print_r($set);
?>
```

Outputs:
Array
(
    [0] => a
    [1] => c
)

Or, to make the array associative with the primary index (code assumes primary index is the first field in the table):


```
<?php
$sql = new MySQLi($host, $username, $password, $database);
$result = $sql->query("SELECT * FROM `$table`;");
for ($set = array (); $row = $result->fetch_assoc(); $set[array_shift($row)] = $row);
print_r($set);
?>
```
<br>Outputs:<br>Array<br>(<br>    [1] =&gt; Array<br>        (<br>            [field2] =&gt; a<br>            [field3] =&gt; b<br>        ),<br>    [2] =&gt; Array<br>        (<br>            [field2] =&gt; c<br>            [field3] =&gt; d<br>        )<br>)  

---

IMPORTANT NOTE:<br><br>If you were used to using code like this:<br><br>

```
<?php
while(false !== ($row = mysql_fetch_assoc($result)))
{
    //...
}
?>
```


You must change it to this for mysqli:



```
<?php
while(null !== ($row = mysqli_fetch_assoc($result)))
{
    //...
}
?>
```
<br><br>The former will cause your script to run until max_execution_time is reached.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-assoc.php)

**[To root](/README.md)**