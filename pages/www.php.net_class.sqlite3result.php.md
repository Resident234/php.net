# The SQLite3Result class



Since SQLite3Result::numRows is unavailable, use:<br><br>

```
<?php
if ($res->numColumns() &amp;&amp; $res->columnType(0) != SQLITE3_NULL) {
    // have rows
} else {
    // zero rows
}
?>
```
<br><br>Because when there are zero rows:<br>* SQLite3Result::fetchArray will return &apos;1&apos;<br>* SQLite3Result::numColumns will return &apos;1&apos;<br>* Column type for column &apos;0&apos; will be SQLITE3_NULL  

---

[Official documentation page](https://www.php.net/manual/en/class.sqlite3result.php)

**[To root](/README.md)**