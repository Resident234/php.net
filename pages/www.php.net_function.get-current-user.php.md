# get_current_user



to get the username of the process owner (rather than the file owner), you can use:<br><br>

```
<?php
$processUser = posix_getpwuid(posix_geteuid());
print $processUser['name'];
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.get-current-user.php)

**[To root](/README.md)**