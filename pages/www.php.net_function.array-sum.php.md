# array_sum



If you want to find the AVERAGE of the values in your array, use the sum and count functions together.  For example, let&apos;s say your array is $foo and you want the average...<br><br>

```
<?php
$average_of_foo = array_sum($foo) / count($foo);
?>
```
  

#

If you want to check if there are for example only strings in an array, you can use a combination of array_sum and array_map like this:<br><br>

```
<?php

function only_strings_in_array($arr) {
    return array_sum(array_map('is_string', $arr)) == count($arr);
}

$arr1 = array('one', 'two', 'three');
$arr2 = array('foo', 'bar', array());
$arr3 = array('foo', array(), 'bar');
$arr4 = array(array(), 'foo', 'bar');

var_dump(
    only_strings_in_array($arr1),
    only_strings_in_array($arr2),
    only_strings_in_array($arr3),
    only_strings_in_array($arr4)
);
?>
```
<br><br>This will give you the following result:<br>bool(true)<br>bool(false)<br>bool(false)<br>bool(false)  

#

[Official documentation page](https://www.php.net/manual/en/function.array-sum.php)

**[To root](/README.md)**