# intdiv



This does indeed seem to be equal to intdiv:<br><br>

```
<?php
function intdiv_1($a, $b){
    return ($a - $a % $b) / $b;
}
?>
```


However, this isn't:



```
<?php
function intdiv_2($a, $b){
    return floor($a / $b);
}
?>
```


Consider an example where either of the parameters is negative:


```
<?php
$param1 = -10;
$param2 = 3;
print_r([
    'modulus' => intdiv_1($param1, $param2),
    'floor' => intdiv_2($param1, $param2),
]);

/**
 * Array
 * (
 *     [modulus] => -3
 *     [floor] => -4
 * )
 */
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.intdiv.php)

**[To root](/README.md)**