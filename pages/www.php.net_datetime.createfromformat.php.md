# DateTime::createFromFormat



Be warned that DateTime object created without explicitely providing the time portion will have the current time set instead of 00:00:00.<br><br>

```
<?php
$date = DateTime::createFromFormat('Y-m-d', '2012-10-17');
var_dump($date->format('Y-m-d H:i:s')); //will print 2012-10-17 13:57:34 (the current time)
?>
```


That's also why you can't safely compare equality of such DateTime objects:



```
<?php
$date1 = DateTime::createFromFormat('Y-m-d', '2012-10-17');
sleep(2);
$date2 = DateTime::createFromFormat('Y-m-d', '2012-10-17');
var_dump($date1 == $date2); //will be false
var_dump($date1 >= $date2); //will be false
var_dump($date1 < $date2); //will be true
?>
```
  

#

If you want to safely compare equality of a DateTime object without explicitly providing the time portion make use of the ! format character.<br><br>

```
<?php
$date1 = DateTime::createFromFormat('!Y-m-d', '2012-10-17');
sleep(2);
$date2 = DateTime::createFromFormat('!Y-m-d', '2012-10-17');
/* 
 $date1 and $date2 will both be set to a timestamp of "2012-10-17 00:00:00"
 var_dump($date1 == $date2); //will be true
 var_dump($date1 > $date2); //will be false
 var_dump($date1 < $date2); //will be false
*/
?>
```


If you omit the ! format character without explicitly providing the time portion your timestamp which will include the current system time in the stamp.



```
<?php
$date1 = DateTime::createFromFormat('Y-m-d', '2012-10-17');
sleep(2);
$date2 = DateTime::createFromFormat('Y-m-d', '2012-10-17');
var_dump($date1 == $date2); //will be false
var_dump($date1 >= $date2); //will be false
var_dump($date1 < $date2); //will be true
?>
```
  

#

Parsing RFC3339 strings can be very tricky when their are microseconds in the date string.<br><br>Since PHP 7 there is the undocumented constant DateTime::RFC3339_EXTENDED (value: Y-m-d\TH:i:s.vP), which can be used to output an RFC3339 string with microseconds:<br><br>

```
<?php
$d = new DateTime();
var_dump($d->format(DateTime::RFC3339_EXTENDED)); // 2017-07-25T13:47:12.000+00:00
?>
```


But the same constant can't be used for parsing an RFC3339 string with microseconds, instead do:



```
<?php
$date = DateTime::createFromFormat("Y-m-d\TH:i:s.uP", "2017-07-25T15:25:16.123456+02:00")
?>
```
<br><br>But "u" can only parse microseconds up to 6 digits, but some language (like Go) return more than 6 digits for the microseconds, e.g.: "2017-07-25T15:50:42.456430712+02:00" (when turning time.Time to JSON with json.Marshal()). Currently there is no other solution than using a separate parsing library to get correct dates.<br><br>Note: the difference between "v" and "u" is just 3 digits vs. 6 digits.  

#

Say if there is a string with  $date = "today is 2014 January 1";   and you need to extract "2014 January" using DateTime::createFromFormat().  As you can see in the string there is something odd like "today is" .Since that string (today is) does not correspond to a date format, we need to escape that. <br><br>In this case, each and every character on that string has to be escaped as shown below.<br><br>The code.<br><br>

```
<?php
$paragraph = "today is 2014 January 1";
$date = DateTime::createFromFormat('\t\o\d\a\y \i\s Y F j', $paragraph);
echo $date->format('Y F'); //"prints" 2014 January

- Shankar Damodaran?>
```
  

#

createFromFormat(&apos;U&apos;) has a strange behaviour: it ignores the datetimezone and the resulting DateTime object will always have GMT+0000 timezone.<br><br>

```
<?php

$dt = DateTime::createFromFormat('U', time(), new DateTimeZone('CET'));
var_dump($dt->format('Y-m-d H:i:s (T)'), date('Y-m-d H:i:s (T)', time()));

?>
```


The problem is microtime() and time() returning the timestamp in current timezone.  Instead of using time you can use 'now' but to get a DateTimeObject with microseconds you have to write it this way to be sure to get the correct datetime:



```
<?php

$dt = \DateTime::createFromFormat('U.u', microtime(true))->setTimezone(new \DateTimeZone(date('T')));

?>
```
  

#

Reportedly, microtime() may return a timestamp number without a fractional part if the microseconds are exactly zero.  I.e., "1463772747" instead of the expected "1463772747.000000".  number_format() can create a correct string representation of the microsecond timestamp every time, which can be useful for creating DateTime objects when used with DateTime::createFromFormat():<br><br>

```
<?php
$now = DateTime::createFromFormat('U.u', number_format(microtime(true), 6, '.', ''));
var_dump($now->format('Y-m-d H:i:s.u')); // E.g., string(26) "2016-05-20 19:36:26.900794"?>
```
  

#

If you&apos;re here because you&apos;re trying to create a date from a week number, you want to be using setISODate, as I discovered here:<br><br>http://www.lornajane.net/posts/2011/getting-dates-from-week-numbers-in

```
<?php?>
```
  

#

It can be confusing creating new DateTime from timestamp when your default timezone (date.timezone) is different from UTC and you are used to date()-function.<br><br>date()-function automatically uses your current timezone setting but DateTime::createFromFormat (or DateTime constructor) does not (it ignores tz-parameter).<br><br>You can get same results as date() by setting the timezone after object creation.<br><br>

```
<?php
$ts = 1414706400;
$date1 = date("Y-m-d H:i", $ts);
$date2 = DateTime::createFromFormat("U", $ts)->setTimeZone(new DateTimeZone(date_default_timezone_get()))->format("Y-m-d H:i");
//$date1===$date2
?>
```
  

#

It seems that a pipe (&apos;|&apos;) option in formating string works only with PHP version 5.3.7 and newer. We had an issue with it on versions 5.3.2, 5.3.3, 5.3.6. Yet it was fine with 5.3.8 and 5.3.10.<br><br>By short example:<br>

```
<?php
$timezone = new DateTimeZone('UTC');
$dateTime = DateTime::createFromFormat('dmY|', '01011972', $timezone);
//$dateTime is FALSE in PHP v <5.3.8
?>
```


Instead we used a workaround:


```
<?php
$dateTime = DateTime::createFromFormat('dmY', '01011972', $timezone);
$dateTime->format('Y-m-d 00:00:00');
?>
```
<br>which works fine.<br><br>====<br><br>Modified by admin to correct for version (5.3.7 not 5.3.8)  

#

I&apos;ve found that on PHP 5.5.13 (not sure if it happens on other versions) if you enter a month larger than 12 on a format that takes numeric months, the result will be a DateTime object with its month equal to the number modulo 12 instead of returning false.<br><br>

```
<?php
var_dump(DateTime::createFromFormat('Y-m-d', '2013-22-01'));
?>
```
<br><br>results in:<br>class DateTime#4 (3) {<br>  public $date =&gt;<br>  string(19) "2014-10-01 13:05:05"<br>  public $timezone_type =&gt;<br>  int(3)<br>  public $timezone =&gt;<br>  string(3) "UTC"<br>}  

#

Not a bug, but a strange issue today 2012-08-30 :<br><br>

```
<?php
$date = "2011-02";
echo $date."\n";
$d = DateTime::createFromFormat("Y-m",$date);
echo $d->format("Y-m");
?>
```

will display : 
2011-02
2011-03

It's because there is no 2011-02-30, so datetime will take march insteed of february ...

To fix it :


```
<?php 
$date = "2011-02";
echo $date."\n";
$d = DateTime::createFromFormat("Y-m-d",$date."-01");
echo $d->format("Y-m");
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.createfromformat.php)

**[To root](/README.md)**