# The InvalidArgumentException class



In my opinion this exception is invaluable for validating arguments- for example providing strict typing a la C:<br><br>

```
<?php
function tripleInteger($int)
{
  if(!is_int($int))
    throw new InvalidArgumentException('tripleInteger function only accepts integers. Input was: '.$int);
  return $int * 3;
}

$x = tripleInteger(4); //$x == 12
$x = tripleInteger(2.5); //exception will be thrown as 2.5 is a float
$x = tripleInteger('foo'); //exception will be thrown as 'foo' is a string
$x = tripleInteger('4'); //exception will throw as '4' is also a string

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.invalidargumentexception.php)

**[To root](/README.md)**