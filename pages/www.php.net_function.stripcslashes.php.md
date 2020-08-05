# stripcslashes



stripcslashes does not simply skip the C-style escape sequences \a, \b, \f, \n, \r, \t and \v, but converts them to their actual meaning. <br><br>So<br>

```
<?php
stripcslashes('\n') == "\n"; //true;

$str = "we are escaping \r\n"; //we are escaping

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.stripcslashes.php)

**[To root](/README.md)**