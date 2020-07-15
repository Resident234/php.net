# The RegexIterator class



An exemple :<br><br>

```
<?php
$a = new ArrayIterator(array('test1', 'test2', 'test3'));
$i = new RegexIterator($a, '/^(test)(\d+)/', RegexIterator::REPLACE);
$i->replacement = '$2:$1';
       
print_r(iterator_to_array($i));
/*
Array
(
    [0] => 1:test
    [1] => 2:test
    [2] => 3:test
)
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.regexiterator.php)

**[To root](/README.md)**