# array_flip



I find this function vey useful when you have a big array and you want to know if a given value is in the array. in_array in fact becomes quite slow in such a case, but you can flip the big array and then use isset to obtain the same result in a much faster way.  

---

This function is useful when parsing a CSV file with a heading column, but the columns might vary in order or presence:<br><br>

```
<?php

$f = fopen("file.csv", "r");

/* Take the first line (the header) into an array, then flip it
so that the keys are the column name, and values are the
column index. */
$cols = array_flip(fgetcsv($f));

while ($line = fgetcsv($f))
{
    // Now we can reference CSV columns like so:
    $status = $line[$cols['OrderStatus']];
}

?>
```
<br><br>I find this better than referencing the numerical array index.  

---

array_flip() does not retain the data type of values, when converting them into keys. :( <br><br>

```
<?php                                                                                                                                                                                                           
$arr = array('one' => '1', 'two' => '2', 'three' => '3');
var_dump($arr);
$arr2 = array_flip($arr);
var_dump($arr2);
?>
```
<br><br>This code outputs this:<br>array(3) {<br>  ["one"]=&gt;<br>  string(1) "1"<br>  ["two"]=&gt;<br>  string(1) "2"<br>  ["three"]=&gt;<br>  string(1) "3"<br>}<br>array(3) {<br>  [1]=&gt;<br>  string(3) "one"<br>  [2]=&gt;<br>  string(3) "two"<br>  [3]=&gt;<br>  string(5) "three"<br>}<br><br>It is valid expectation that string values "1", "2" and "3" would become string keys "1", "2" and "3".  

---

[Official documentation page](https://www.php.net/manual/en/function.array-flip.php)

**[To root](/README.md)**