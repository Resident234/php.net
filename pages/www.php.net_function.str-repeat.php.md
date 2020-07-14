# str_repeat



Here is a simple one liner to repeat a string multiple times with a separator:<br><br>

```
<?php
implode($separator, array_fill(0, $multiplier, $input));
?>
```


Example script:


```
<?php

// How I like to repeat a string using standard PHP functions
$input = &apos;bar&apos;;
$multiplier = 5;
$separator = &apos;,&apos;;
print implode($separator, array_fill(0, $multiplier, $input));
print "\n";

// Say, this comes in handy with count() on an array that we want to use in an
// SQL query such as &apos;WHERE foo IN (...)&apos;
$args = array(&apos;1&apos;, &apos;2&apos;, &apos;3&apos;);
print implode(&apos;,&apos;, array_fill(0, count($args), &apos;?&apos;));
print "\n";
?>
```
<br><br>Example Output:<br>bar,bar,bar,bar,bar<br>?,?,?  

#

[Official documentation page](https://www.php.net/manual/en/function.str-repeat.php)

**[To root](/README.md)**