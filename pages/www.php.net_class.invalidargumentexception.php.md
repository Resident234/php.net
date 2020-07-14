# The InvalidArgumentException class



In my opinion this exception is invaluable for validating arguments- for example providing strict typing a la C:<br><br>

```
<?php
function tripleInteger($int)
{
  if(!is_int($int))
    throw new InvalidArgumentException(&apos;tripleInteger function only accepts integers. Input was: &apos;.$int);
  return $int * 3;
}

$x = tripleInteger(4); //$x == 12
$x = tripleInteger(2.5); //exception will be thrown as 2.5 is a float
$x = tripleInteger(&apos;foo&apos;); //exception will be thrown as &apos;foo&apos; is a string
$x = tripleInteger(&apos;4&apos;); //exception will throw as &apos;4&apos; is also a string

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.invalidargumentexception.php)

**[To root](/README.md)**