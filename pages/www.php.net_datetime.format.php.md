# DateTime::format





Using a datetime field from a mysql database e.g. &quot;2012-03-24 17:45:12&quot;



```
<?php

$result = mysql_query(&quot;SELECT `datetime` FROM `table`&quot;);
$row = mysql_fetch_row($result);
$date = date_create($row[0]);

echo date_format($date, &apos;Y-m-d H:i:s&apos;);
#output: 2012-03-24 17:45:12

echo date_format($date, &apos;d/m/Y H:i:s&apos;);
#output: 24/03/2012 17:45:12

echo date_format($date, &apos;d/m/y&apos;);
#output: 24/03/12

echo date_format($date, &apos;g:i A&apos;);
#output: 5:45 PM

echo date_format($date, &apos;G:ia&apos;);
#output: 05:45pm

echo date_format($date, &apos;g:ia \o\n l jS F Y&apos;);
#output: 5:45pm on Saturday 24th March 2012

?>
```



  

#



For full reference of the supported format character and results,
see the documentation of date() :
http://www.php.net/manual/en/function.date.php

  

#



Seems like datetime::format does not really support microseconds as the documentation under date suggest it will.

Here is some code to generate a datetime with microseconds and timezone:

private function udate($format = &apos;u&apos;, $utimestamp = null) {
&#xA0; &#xA0; &#xA0; &#xA0; if (is_null($utimestamp))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $utimestamp = microtime(true);

&#xA0; &#xA0; &#xA0; &#xA0; $timestamp = floor($utimestamp);
&#xA0; &#xA0; &#xA0; &#xA0; $milliseconds = round(($utimestamp - $timestamp) * 1000000);

&#xA0; &#xA0; &#xA0; &#xA0; return date(preg_replace(&apos;`(?&lt;!\\\\)u`&apos;, $milliseconds, $format), $timestamp);
&#xA0; &#xA0; }

echo udate(&apos;Y-m-d H:i:s.u T&apos;);
// Will output something like: 2014-01-01 12:20:24.42342 CET

  

#

[Official documentation page](https://www.php.net/manual/en/datetime.format.php)

**[To root](/README.md)**