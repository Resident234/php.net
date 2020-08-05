# DateInterval::__construct



M is used to indicate both months and minutes.<br><br>As noted on the referenced wikipedia page for ISO 6801 http://en.wikipedia.org/wiki/Iso8601#Durations<br><br>To resolve ambiguity, "P1M" is a one-month duration and "PT1M" is a one-minute duration (note the time designator, T, that precedes the time value).<br><br>Using: PHP 5.3.2-1ubuntu4.19<br><br>// For 3 Months<br>$dateTime = new DateTime;echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>$dateTime-&gt;add(new DateInterval("P3M"));<br>echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>Results in:<br>2013-07-11T11:12:26-0400<br>2013-10-11T11:12:26-0400<br><br>// For 3 Minutes<br>$dateTime = new DateTime;echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>$dateTime-&gt;add(new DateInterval("PT3M"));<br>echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;<br>Results in:<br>2013-07-11T11:12:42-0400<br>2013-07-11T11:15:42-0400<br><br>Insert a T after the P in the interval to add 3 minutes instead of 3 months.  

---

Note that, while a DateInterval object has an $invert property, you cannot supply a negative directly to the constructor similar to specifying a negative in XSD ("-P1Y"). You will get an exception through if you do this. <br><br>Instead you need to construct using a positive interval ("P1Y") and the specify the $invert property === 1.  

---

I think it is easiest if you would just use the sub method on the DateTime class.<br><br>

```
<?php
$date = new DateTime();
$date->sub(new DateInterval("P89D"));?>
```
  

---

It should be noted that this class will not calculate days/hours/minutes/seconds etc given a value in a single denomination of time.  For example:<br><br>

```
<?php
    $di = new DateInterval('PT3600S');
    echo $di->format('%H:%i:%s');
    
?>
```
<br><br>will yield 0:0:3600 instead of the expected 1:0:0  

---

[Official documentation page](https://www.php.net/manual/en/dateinterval.construct.php)

**[To root](/README.md)**