# array_replace





```
<?php
// we wanted the output of only selected array_keys from a big array from a csv-table
// with different order of keys, with optional suppressing of empty or unused values

$values = array
(
    'Article'=>'24497',
    'Type'=>'LED',
    'Socket'=>'E27',
    'Dimmable'=>'',
    'Wattage'=>'10W'
);

$keys = array_fill_keys(array('Article','Wattage','Dimmable','Type','Foobar'), ''); // wanted array with empty value

$allkeys = array_replace($keys, array_intersect_key($values, $keys));    // replace only the wanted keys

$notempty = array_filter($allkeys, 'strlen'); // strlen used as the callback-function with 0==false

print '<pre>';
print_r($allkeys);
print_r($notempty);

/*
Array
(
    [Article] => 24497
    [Wattage] => 10W
    [Dimmable] => 
    [Type] => LED
    [Foobar] => 
)
Array
(
    [Article] => 24497
    [Wattage] => 10W
    [Type] => LED
)
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-replace.php)

**[To root](/README.md)**