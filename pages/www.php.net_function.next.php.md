# next



Now from PHP 7.2, the function "each" is deprecated, so the has_next I&apos;ve posted is no longer a good idea. There is another to keep it simple and fast:<br><br>

```
<?php
function has_next(array $_array)
{
  return next($_array) !== false ?: key($_array) !== null;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.next.php)

**[To root](/README.md)**