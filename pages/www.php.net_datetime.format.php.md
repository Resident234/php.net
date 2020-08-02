# DateTime::format



Using a datetime field from a mysql database e.g. "2012-03-24 17:45:12"<br><br>

```
<?php

$result = mysql_query("SELECT `datetime` FROM `table`");
$row = mysql_fetch_row($result);
$date = date_create($row[0]);

echo date_format($date, 'Y-m-d H:i:s');
#output: 2012-03-24 17:45:12

echo date_format($date, 'd/m/Y H:i:s');
#output: 24/03/2012 17:45:12

echo date_format($date, 'd/m/y');
#output: 24/03/12

echo date_format($date, 'g:i A');
#output: 5:45 PM

echo date_format($date, 'G:ia');
#output: 05:45pm

echo date_format($date, 'g:ia \o\n l jS F Y');
#output: 5:45pm on Saturday 24th March 2012

?>
```
  

---

For full reference of the supported format character and results,<br>see the documentation of date() :<br>http://www.php.net/manual/en/function.date.php  

---

Seems like datetime::format does not really support microseconds as the documentation under date suggest it will.<br><br>Here is some code to generate a datetime with microseconds and timezone:<br><br>private function udate($format = &apos;u&apos;, $utimestamp = null) {<br>        if (is_null($utimestamp))<br>            $utimestamp = microtime(true);<br><br>        $timestamp = floor($utimestamp);<br>        $milliseconds = round(($utimestamp - $timestamp) * 1000000);<br><br>        return date(preg_replace(&apos;`(?&lt;!\\\\)u`&apos;, $milliseconds, $format), $timestamp);<br>    }<br><br>echo udate(&apos;Y-m-d H:i:s.u T&apos;);<br>// Will output something like: 2014-01-01 12:20:24.42342 CET  

---

[Official documentation page](https://www.php.net/manual/en/datetime.format.php)

**[To root](/README.md)**