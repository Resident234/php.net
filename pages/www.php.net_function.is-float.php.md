# is_float



Coercing the value to float and back to string was a neat trick. You can also just add a literal 0 to whatever you&apos;re checking.<br><br>

```
<?php
function isfloat($value) {
  // PHP automagically tries to coerce $value to a number
  return is_float($value + 0);
}
?>
```


Seems to work ok:



```
<?php
isfloat("5.0" + 0);  // true
isfloat("5.0");  // false
isfloat(5 + 0);  // false
isfloat(5.0 + 0);  // false
isfloat(&apos;a&apos; + 0);  // false
?>
```
<br><br>YMMV  

#

If you want to test whether a string is containing a float, rather than if a variable is a float, you can use this simple little function:<br><br>function isfloat($f) return ($f == (string)(float)$f);  

#

[Official documentation page](https://www.php.net/manual/en/function.is-float.php)

**[To root](/README.md)**