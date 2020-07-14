# Sorting Arrays



While this may seem obvious, user-defined array sorting functions ( uksort(), uasort(), usort() ) will *not* be called if the array does not have *at least two values in it*.<br><br>The following code:                        <br><br>

```
<?php

function usortTest($a, $b) {
    var_dump($a);
    var_dump($b);
    return -1;
}

$test = array(&apos;val1&apos;);
usort($test, "usortTest");

$test2 = array(&apos;val2&apos;, &apos;val3&apos;);
usort($test2, "usortTest");

?>
```
<br><br>Will output: <br><br>string(4) "val3"<br>string(4) "val2"<br><br>The first array doesn&apos;t get sent to the function.<br><br>Please, under no circumstance, place any logic that modifies values, or applies non-sorting business logic in these functions as they will not always be executed.  

#

Another way to do a case case-insensitive sort by key would simply be:<br><br>

```
<?php
uksort($array, &apos;strcasecmp&apos;);
?>
```
<br><br>Since strcasecmp is already predefined in php it saves you the trouble to actually write the comparison function yourself.  

#

[Official documentation page](https://www.php.net/manual/en/array.sorting.php)

**[To root](/README.md)**