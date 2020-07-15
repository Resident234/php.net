# mysqli::get_warnings



With a bit of rooting about with reflection, I spotted that the mysqli_warning class has a next() function, so I tried calling it and it does indeed progress through the available warnings! Following on from my earlier example:<br><br>

```
<?php
$r = mysqli_query($db, "INSERT INTO blah SET z = '1'");
$j = mysqli_warning_count($db);
if ($j &gt; 0) {
    $e = mysqli_get_warnings($db);
    for ($i = 0; $i &lt; $j; $i++) {
        var_dump($e);
        $e->next();
    }
}
?>
```


There is a simple way of traversing the warnings:



```
<?php
$r = mysqli_query($db, "INSERT INTO blah SET z = '1'");
if (mysqli_warning_count($db)) {
   $e = mysqli_get_warnings($db);
   do {
       echo "Warning: $e->errno: $e->message\n";
   } while ($e->next());
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.get-warnings.php)

**[To root](/README.md)**