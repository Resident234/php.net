# DatePeriod::__construct



I found two things useful to know that aren&apos;t covered here.<br><br>1. endDate is excluded:<br><br>

```
<?php
$i = new DateInterval('P1D');
$d1 = new Datetime();
$d2 = clone $d1; $d2->add($i);
foreach(new DatePeriod($d1, $i, $d2) as $d) {
    echo $d->format('Y-m-d H:i:s') . "\n";
}
?>
```


Will output:
2010-11-03 12:39:53

(Another one because I got it wrong at first)
2. For the first form, recurrences really means REcurrences, not occurences.



```
<?php
$i = new DateInterval('P1D');
$d = new Datetime();
foreach(new DatePeriod($d, $i, 1) as $d) {
    echo $d->format('Y-m-d H:i:s') . "\n";
}
?>
```
<br><br>Will output:<br>2010-11-03 12:41:05<br>2010-11-04 12:41:05  

---

When you add the time 23:59:59 to the end DateTime object something like the following then the end date will be included in the period:<br><br>

```
<?php
$date_start = new DateTime('2012-03-12');
$date_end = new DateTime('2012-03-22 23:59:59');

$interval = '+2 days';
$date_interval = DateInterval::createFromDateString($interval);

$period = new DatePeriod($date_start, $date_interval, $date_end, DatePeriod::EXCLUDE_START_DATE);

foreach($period as $dt) {
 echo $dt->format('d/m');
}
?>
```
<br><br>OUTPUT:<br>14/03<br>16/03<br>18/03<br>20/03<br>22/03  

---

[Official documentation page](https://www.php.net/manual/en/dateperiod.construct.php)

**[To root](/README.md)**