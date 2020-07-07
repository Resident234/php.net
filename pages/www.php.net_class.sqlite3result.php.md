# The SQLite3Result class





Since SQLite3Result::numRows is unavailable, use:





```
<?php

if ($res-&gt;numColumns() &amp;&amp; $res-&gt;columnType(0) != SQLITE3_NULL) {

&#xA0; &#xA0; // have rows

} else {

&#xA0; &#xA0; // zero rows

}

?>
```




Because when there are zero rows:

* SQLite3Result::fetchArray will return &apos;1&apos;

* SQLite3Result::numColumns will return &apos;1&apos;

* Column type for column &apos;0&apos; will be SQLITE3_NULL

  

#

[Official documentation page](https://www.php.net/manual/en/class.sqlite3result.php)

**[To root](/README.md)**