# DateTime::__construct



There&apos;s a reason for ignoring the time zone when you pass a timestamp to __construct.  That is, UNIX timestamps are by definition based on UTC.  @1234567890 represents the same date/time regardless of time zone.  So there&apos;s no need for a time zone at all.  

---

The theoretical limits of the date range seem to be "-9999-01-01" through "9999-12-31" (PHP 5.2.9 on Windows Vista 64):<br><br>

```
<?php

$d = new DateTime("9999-12-31"); 
$d->format("Y-m-d"); // "9999-12-31"

$d = new DateTime("0000-12-31"); 
$d->format("Y-m-d"); // "0000-12-31"

$d = new DateTime("-9999-12-31"); 
$d->format("Y-m-d"); // "-9999-12-31"

?>
```


Dates above 10000 and below -10000 do not throw errors but produce weird results:



```
<?php

$d = new DateTime("10019-01-01"); 
$d->format("Y-m-d"); // "2009-01-01"

$d = new DateTime("10009-01-01"); 
$d->format("Y-m-d"); // "2009-01-01"

$d = new DateTime("-10019-01-01"); 
$d->format("Y-m-d"); // "2009-01-01"

?>
```
  

---

A definite "gotcha" (while documented) that exists in the __construct is that it ignores your timezone if the $time is a timestamp.  While this may not make sense, the object does provide you with methods to work around it.<br><br>

```
<?php
// New Timezone Object
$timezone = new DateTimeZone('America/New_York');

// New DateTime Object
$date =  new DateTime('@1306123200', $timezone);    

// You would expect the date to be 2011-05-23 00:00:00
// But it actually outputs 2011-05-23 04:00:00
echo $date->format('Y-m-d H:i:s');

// You can still set the timezone though like so...        
$date->setTimezone($timezone);

// This will now output 2011-05-23 00:00:00
echo $date->format('Y-m-d H:i:s');
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/datetime.construct.php)

**[To root](/README.md)**