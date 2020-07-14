# getdate



Andre&apos;s code will throw an error. for the following line<br>    <br>     $d = $todayh[mday];<br>     $m = $todayh[mon];<br>     $y = $todayh[year];<br><br>"Notice : Undefined constant mday ,mon,year"<br><br>As is, it was looking for constants called mday, mon, year etc. When it doesn&apos;t find such a constant, PHP interprets it as a string. <br><br>like any other request it should be wrapped in quotes like this<br><br>     $d = $todayh[&apos;mday&apos;];<br>     $m = $todayh[&apos;mon&apos;];<br>     $y = $todayh[&apos;year&apos;];  

#

In addition to canby23 at ms19 post:<br>It&apos;s a very bad idea to consider day having 24 hours (86400 secs), because some days have 23, some - 25 hours due to daylight saving changes. Using of mkdate() and strtotime() is always preferred. strtotime() also has a very nice behaviour of datetime manipulations:<br>

```
<?php
echo strtotime ("+1 day"), "\n";
echo strtotime ("+1 week"), "\n";
echo strtotime ("+1 week 2 days 4 hours 2 seconds"), "\n";
echo strtotime ("next Thursday"), "\n";
echo strtotime ("last Monday"), "\n"; 
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.getdate.php)

**[To root](/README.md)**