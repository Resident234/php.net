# array_product



This function can be used to test if all values in an array of booleans are TRUE.<br><br>Consider:<br><br>

```
<?php

function outbool($test)
{
    return (bool) $test;
}

$check[] = outbool(TRUE);
$check[] = outbool(1);
$check[] = outbool(FALSE);
$check[] = outbool(0);

$result = (bool) array_product($check);
// $result is set to FALSE because only two of the four values evaluated to TRUE

?>
```


The above is equivalent to:



```
<?php

$check1 = outbool(TRUE);
$check2 = outbool(1);
$check3 = outbool(FALSE);
$check4 = outbool(0);

$result = ($check1 &amp;&amp; $check2 &amp;&amp; $check3 &amp;&amp; $check4);

?>
```
<br><br>This use of array_product is especially useful when testing an indefinite number of booleans and is easy to construct in a loop.  

#

[Official documentation page](https://www.php.net/manual/en/function.array-product.php)

**[To root](/README.md)**