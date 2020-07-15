# DateTime::setTimestamp



It should be noted above, be careful when manipulating the DateTime object with unix timestamps.<br>In the above examples you will get varying results dependent on your current timezone, method used, and version of PHP.<br><br>One would expect all of the examples above to perform the same as setTimestamp() or date(&apos;H:i&apos;, $timestamp); would.<br><br>

```
<?php
date_default_timezone_set('America/New_York');

$ts = 1171502725;
?>
```


Set timestamp from UTC timezone use UTC timezone


```
<?php
$date = new DateTime("@$ts"); 
var_dump($date->format('Y-m-d H:i:s e'));
/*
string(26) "2007-02-15 01:25:25 +00:00" //PHP 5.3.0 - 5.6.8
*/
?>
```


To convert the above to use the current timezone simply use


```
<?php
$date->setTimezone(date_default_timezone_get());
//string(36) "2007-02-14 20:25:25 America/New_York"
?>
```


Set the timestamp from UTC timezone use current timezone


```
<?php
$date = new DateTime;
$date->modify('@' . $ts); 
var_dump($date->format('Y-m-d H:i:s e'));
/*
string(36) "2007-02-15 01:25:25 America/New_York" //PHP 5.3.6 - 5.6.8
string(36) "2052-06-20 18:53:24 America/New_York" //PHP 5.3.0 - 5.3.5
*/
?>
```


Set the timestamp from current timezone use current timezone


```
<?php
$date = new DateTime;
$date->setTimestamp($ts); 
var_dump($date->format('Y-m-d H:i:s e'));
/*
string(36) "2007-02-14 20:25:25 America/New_York" //PHP 5.3.0 - 5.6.8
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.settimestamp.php)

**[To root](/README.md)**