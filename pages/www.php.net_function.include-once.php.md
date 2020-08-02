# include_once



In response to what a user wrote 8 years ago regarding include_once being ran two times in a row on a non-existent file:<br><br>Perhaps 8 years ago that was the case, however I have tested in PHP 5.6, and I get this:<br><br>$result = include_once &apos;fakefile.php&apos;;  // $result = false<br>$result = include_once &apos;fakefile.php&apos;   // $result is still false  

---

If you include a file that does not exist with include_once, the return result will be false. <br><br>If you try to include that same file again with include_once the return value will be true.<br><br>Example:<br>

```
<?php
var_dump(include_once 'fakefile.ext'); // bool(false)
var_dump(include_once 'fakefile.ext'); // bool(true)
?>
```
<br><br>This is because according to php the file was already included once (even though it does not exist).  

---

[Official documentation page](https://www.php.net/manual/en/function.include-once.php)

**[To root](/README.md)**