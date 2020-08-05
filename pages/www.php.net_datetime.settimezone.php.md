# DateTime::setTimezone



In response to the other comments expressing surprise that changing the timezone does not affect the timestamp:<br><br>A UNIX timestamp is defined as the number of seconds that have elapsed since 00:00:00 (UTC), Thursday, 1 January 1970.<br><br>So: with respect to UTC. Always.<br><br>Calling setTimezone() never changes the actual "absolute", underlying, moment-in-time itself. It only changes the timezone you wish to "view" that moment "from". Consider the following:<br><br>

```
<?php
// A time in London.
$datetime = new DateTime('2015-06-22T10:40:25', new DateTimeZone('Europe/London'));

// I wonder how that SAME moment-in-time would 
// be described in other places around the world.
$datetime->setTimezone(new DateTimeZone('Australia/Sydney'));
print $datetime->format('Y-m-d H:i:s (e)');
  // 2015-06-22 19:40:25 (Australia/Sydney)

$datetime->setTimezone(new DateTimeZone('America/New_York'));
print $datetime->format('Y-m-d H:i:s (e)');
  // 2015-06-22 05:40:25 (America/New_York)

$datetime->setTimezone(new DateTimeZone('Asia/Calcutta'));
print $datetime->format('Y-m-d H:i:s (e)');
  // 2015-06-22 15:10:25 (Asia/Calcutta)
?>
```


Please note that ALL of these date strings unambiguously represent the exact same moment-in-time. Therefore, calling getTimestamp() at any stage will return the same result:



```
<?php
$datetime->getTimestamp();
  // 1434966025
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/datetime.settimezone.php)

**[To root](/README.md)**