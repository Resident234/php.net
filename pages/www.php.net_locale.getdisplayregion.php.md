# Locale::getDisplayRegion



You don&apos;t have to have a fully-formed locale for the first parameter. This makes the function useful for getting the country name from any locale:<br><br>

```
<?php
var_dump(Locale::getDisplayRegion('-US', 'fr'));

//Returns
string '&#xC9;tats-Unis' (length=11)
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/locale.getdisplayregion.php)

**[To root](/README.md)**