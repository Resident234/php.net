# ArgumentCountError



Note if an invalid number of arguments are passed to a built-in function an ArgumentCountError exception will be thrown if and only if your code is in strict mode.<br><br>

```
<?php
declare(strict_types = 1);

try {
    echo strlen('ahmed', 4);
} catch (ArgumentCountError $e) {
    echo $e->getMessage()';
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.argumentcounterror.php)

**[To root](/README.md)**