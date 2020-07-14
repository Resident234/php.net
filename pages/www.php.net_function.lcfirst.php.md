# lcfirst



Easiest work-around I&apos;ve found for &lt;5.3:<br>

```
<?php

$string = "CamelCase"
$string{0} = strtolower($string{0})
echo $string; // outputs camelCase

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.lcfirst.php)

**[To root](/README.md)**