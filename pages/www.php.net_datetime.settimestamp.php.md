# DateTime::setTimestamp





It should be noted above, be careful when manipulating the DateTime object with unix timestamps.
In the above examples you will get varying results dependent on your current timezone, method used, and version of PHP.

One would expect all of the examples above to perform the same as setTimestamp() or date(&apos;H:i&apos;, $timestamp); would.



```
<?php
date_default_timezone_set(&apos;America/New_York&apos;);

$ts = 1171502725;
?>
```


Set timestamp from UTC timezone use UTC timezone


```
<?php
$date = new DateTime(&quot;@$ts&quot;); 
var_dump($date-&gt;format(&apos;Y-m-d H:i:s e&apos;));
/*
string(26) &quot;2007-02-15 01:25:25 +00:00&quot; //PHP 5.3.0 - 5.6.8
*/
?>
```


To convert the above to use the current timezone simply use


```
<?php
$date-&gt;setTimezone(date_default_timezone_get());
//string(36) &quot;2007-02-14 20:25:25 America/New_York&quot;
?>
```


Set the timestamp from UTC timezone use current timezone


```
<?php
$date = new DateTime;
$date-&gt;modify(&apos;@&apos; . $ts); 
var_dump($date-&gt;format(&apos;Y-m-d H:i:s e&apos;));
/*
string(36) &quot;2007-02-15 01:25:25 America/New_York&quot; //PHP 5.3.6 - 5.6.8
string(36) &quot;2052-06-20 18:53:24 America/New_York&quot; //PHP 5.3.0 - 5.3.5
*/
?>
```


Set the timestamp from current timezone use current timezone


```
<?php
$date = new DateTime;
$date-&gt;setTimestamp($ts); 
var_dump($date-&gt;format(&apos;Y-m-d H:i:s e&apos;));
/*
string(36) &quot;2007-02-14 20:25:25 America/New_York&quot; //PHP 5.3.0 - 5.6.8
*/
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/datetime.settimestamp.php)

**[To root](/README.md)**