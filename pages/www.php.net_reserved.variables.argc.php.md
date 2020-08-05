# $argc



To find out are you in CLI or not, this is much better in my opinion:<br>

```
<?php
if (PHP_SAPI != "cli") {
    exit;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.argc.php)

**[To root](/README.md)**