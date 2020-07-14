# DateTime::getTimestamp



Note that for dates before the unix epoch getTimestamp() will return false, whereas format("U") will return a negative number.<br><br>

```
<?php
$date = new DateTime("1899-12-31");
// "-2209078800"
echo $date-&gt;format("U");
// false
echo $date-&gt;getTimestamp();
?>
```
  

#

In 32-bit system the unix timestamp will overflow if the date goes beyond year 2038 and this method will return false. In 64-bit systems this function will still work as intended. For more information please see http://en.wikipedia.org/wiki/Year_2038_problem.  

#

[Official documentation page](https://www.php.net/manual/en/datetime.gettimestamp.php)

**[To root](/README.md)**