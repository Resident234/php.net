# array_replace





```
<?php
// we wanted the output of only selected array_keys from a big array from a csv-table
// with different order of keys, with optional suppressing of empty or unused values

$values = array
(
    &apos;Article&apos;=&gt;&apos;24497&apos;,
    &apos;Type&apos;=&gt;&apos;LED&apos;,
    &apos;Socket&apos;=&gt;&apos;E27&apos;,
    &apos;Dimmable&apos;=&gt;&apos;&apos;,
    &apos;Wattage&apos;=&gt;&apos;10W&apos;
);

$keys = array_fill_keys(array(&apos;Article&apos;,&apos;Wattage&apos;,&apos;Dimmable&apos;,&apos;Type&apos;,&apos;Foobar&apos;), &apos;&apos;); // wanted array with empty value

$allkeys = array_replace($keys, array_intersect_key($values, $keys));    // replace only the wanted keys

$notempty = array_filter($allkeys, &apos;strlen&apos;); // strlen used as the callback-function with 0==false

print &apos;&lt;pre&gt;&apos;;
print_r($allkeys);
print_r($notempty);

/*
Array
(
    [Article] =&gt; 24497
    [Wattage] =&gt; 10W
    [Dimmable] =&gt; 
    [Type] =&gt; LED
    [Foobar] =&gt; 
)
Array
(
    [Article] =&gt; 24497
    [Wattage] =&gt; 10W
    [Type] =&gt; LED
)
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-replace.php)

**[To root](/README.md)**