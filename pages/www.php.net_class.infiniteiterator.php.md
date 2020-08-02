# The InfiniteIterator class



to loop through object keys and reset to the start, try this:<br>

```
<?php

$obj = new stdClass();
$obj->Mon = "Monday";
$obj->Tue = "Tuesday";
$obj->Wed = "Wednesday";
$obj->Thu = "Thursday";
$obj->Fri = "Friday";
$obj->Sat = "Saturday";
$obj->Sun = "Sunday";

$infinate = new InfiniteIterator(new ArrayIterator($obj));
foreach ( new LimitIterator($infinate, 0, 14) as $value ) {
    print($value . PHP_EOL);
}

?>
```
<br><br>will output:<br><br>Monday<br>Tuesday<br>Wednesday<br>Thursday<br>Friday<br>Saturday<br>Sunday<br>Monday<br>Tuesday<br>Wednesday<br>Thursday<br>Friday<br>Saturday<br>Sunday<br><br>Can be useful when doing date operations or recurring events  

---

It is important to realise that rewind() must be called on any iterator before using it or you may experience undefined behaviour, see example code and output here http://3v4l.org/rvNpU<br><br>See this bug report https://bugs.php.net/bug.php?id=63823&amp;edit=2 for a fuller explanation.  

---

[Official documentation page](https://www.php.net/manual/en/class.infiniteiterator.php)

**[To root](/README.md)**