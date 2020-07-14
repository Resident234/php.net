# is_numeric



If you want the numerical value of a string, this will return a float or int value:<br><br>

```
<?php
function get_numeric($val) {
  if (is_numeric($val)) {
    return $val + 0;
  }
  return 0;
}
?>
```


Example:


```
<?php
get_numeric(&apos;3&apos;); // int(3)
get_numeric(&apos;1.2&apos;); // float(1.2)
get_numeric(&apos;3.0&apos;); // float(3)
?>
```
  

#

Note that the function accepts extremely big numbers and correctly evaluates them.<br><br>For example:<br><br>

```
<?php
    $v = is_numeric (&apos;58635272821786587286382824657568871098287278276543219876543&apos;) ? true : false;
    
    var_dump ($v);
?>
```
<br><br>The above script will output:<br><br>bool(true)<br><br>So this function is not intimidated by super-big numbers. I hope this helps someone.<br><br>PS: Also note that if you write is_numeric (45thg), this will generate a parse error (since the parameter is not enclosed between apostrophes or double quotes). Keep this in mind when you use this function.  

#

The documentation does not clarify what happens if you the input is an empty string - it correctly returns false in my experience.  Useful to state these odd cases, for when you see code that checks for an empty string and is_numeric, you can tell it&apos;s a waste of a comparison.  

#

for strings, it return true only if float number has a dot<br><br>is_numeric( &apos;42.1&apos; )//true<br>is_numeric( &apos;42,1&apos; )//false  

#

The documentation is not completely precise here. is_numeric will also return true if the number begins with a decimal point  and/or a space, provided a number follows (rather than a letter or punctuation). So, it doesn&apos;t necessarily have to start with a digit.  

#

[Official documentation page](https://www.php.net/manual/en/function.is-numeric.php)

**[To root](/README.md)**