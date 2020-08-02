# substr_compare



When you came to this page, you may have been looking for something a little simpler: A function that can check if a small string exists within a larger string starting at a particular index. Using substr_compare() for this can leave your code messy, because you need to check that your string is long enough (to avoid the warning), manually specify the length of the short string, and like many of the string functions, perform an integer comparison to answer a true/false question.<br><br>I put together a simple function to return true if $str exists within $mainStr. If $loc is specified, the $str must begin at that index. If not, the entire $mainStr will be searched.<br><br>

```
<?php

function contains_substr($mainStr, $str, $loc = false) {
    if ($loc === false) return (strpos($mainStr, $str) !== false);
    if (strlen($mainStr) < strlen($str)) return false;
    if (($loc + strlen($str)) > strlen($mainStr)) return false;
    return (strcmp(substr($mainStr, $loc, strlen($str)), $str) == 0);
}

?>
```
  

---

Take note of the `length` parameter: "The default value is the largest of the length of the str compared to the length of main_str less the offset."<br><br>This is *not* the length of str as you might (I always) expect, so if you leave it out, you&apos;ll get unexpected results.  Example:<br><br>

```
<?php
$hash = '$5$lalalalalalalala$crypt.output.here';
var_dump(substr_compare($hash, '$5  , 0)); # int(34)
var_dump(substr_compare($hash, '$5  , 0, 3)); # int(0)
var_dump(PHP_VERSION); # string(6) "5.3.14"
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.substr-compare.php)

**[To root](/README.md)**