# DateTime::getLastErrors



DateTime::createFromFormat is smart to handle the cases where you input an invalid date, like April 31st, and convert it to May 1st. In some cases, you do not want this automatic smart handling of the dates for example in a user input form where you want to be sure that your user did input the date he wanted. To do that, you need to get access to the warnings, this method is the only way to do it:<br><br>

```
<?php
$date = DateTime::createFromFormat('Y-m-d', '1999-04-31');
print $date->format('Y-m-d') . PHP_EOL;
print_r(DateTime::getLastErrors());
?>
```
<br><br>The output is:<br><br>1999-05-01<br>Array<br>(<br>    [warning_count] =&gt; 1<br>    [warnings] =&gt; Array<br>        (<br>            [10] =&gt; The parsed date was invalid<br>        )<br><br>    [error_count] =&gt; 0<br>    [errors] =&gt; Array<br>        (<br>        )<br><br>)<br><br>So, here you can see, you have a warning because the date was invalid, but not an error because PHP was smart enough to convert it into a valid date. It is then up to you to do something with this information.  

---

[Official documentation page](https://www.php.net/manual/en/datetime.getlasterrors.php)

**[To root](/README.md)**