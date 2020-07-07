# DateTime::createFromFormat





Be warned that DateTime object created without explicitely providing the time portion will have the current time set instead of 00:00:00.



```
<?php
$date = DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;2012-10-17&apos;);
var_dump($date-&gt;format(&apos;Y-m-d H:i:s&apos;)); //will print 2012-10-17 13:57:34 (the current time)
?>
```


That&apos;s also why you can&apos;t safely compare equality of such DateTime objects:



```
<?php
$date1 = DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;2012-10-17&apos;);
sleep(2);
$date2 = DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;2012-10-17&apos;);
var_dump($date1 == $date2); //will be false
var_dump($date1 &gt;= $date2); //will be false
var_dump($date1 &lt; $date2); //will be true
?>
```



  

#



If you want to safely compare equality of a DateTime object without explicitly providing the time portion make use of the ! format character.



```
<?php
$date1 = DateTime::createFromFormat(&apos;!Y-m-d&apos;, &apos;2012-10-17&apos;);
sleep(2);
$date2 = DateTime::createFromFormat(&apos;!Y-m-d&apos;, &apos;2012-10-17&apos;);
/* 
 $date1 and $date2 will both be set to a timestamp of &quot;2012-10-17 00:00:00&quot;
 var_dump($date1 == $date2); //will be true
 var_dump($date1 &gt; $date2); //will be false
 var_dump($date1 &lt; $date2); //will be false
*/
?>
```


If you omit the ! format character without explicitly providing the time portion your timestamp which will include the current system time in the stamp.



```
<?php
$date1 = DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;2012-10-17&apos;);
sleep(2);
$date2 = DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;2012-10-17&apos;);
var_dump($date1 == $date2); //will be false
var_dump($date1 &gt;= $date2); //will be false
var_dump($date1 &lt; $date2); //will be true
?>
```



  

#



Parsing RFC3339 strings can be very tricky when their are microseconds in the date string.

Since PHP 7 there is the undocumented constant DateTime::RFC3339_EXTENDED (value: Y-m-d\TH:i:s.vP), which can be used to output an RFC3339 string with microseconds:



```
<?php
$d = new DateTime();
var_dump($d-&gt;format(DateTime::RFC3339_EXTENDED)); // 2017-07-25T13:47:12.000+00:00
?>
```


But the same constant can&apos;t be used for parsing an RFC3339 string with microseconds, instead do:



```
<?php
$date = DateTime::createFromFormat(&quot;Y-m-d\TH:i:s.uP&quot;, &quot;2017-07-25T15:25:16.123456+02:00&quot;)
?>
```


But &quot;u&quot; can only parse microseconds up to 6 digits, but some language (like Go) return more than 6 digits for the microseconds, e.g.: &quot;2017-07-25T15:50:42.456430712+02:00&quot; (when turning time.Time to JSON with json.Marshal()). Currently there is no other solution than using a separate parsing library to get correct dates.

Note: the difference between &quot;v&quot; and &quot;u&quot; is just 3 digits vs. 6 digits.

  

#



Say if there is a string with&#xA0; $date = &quot;today is 2014 January 1&quot;;&#xA0;&#xA0; and you need to extract &quot;2014 January&quot; using DateTime::createFromFormat().&#xA0; As you can see in the string there is something odd like &quot;today is&quot; .Since that string (today is) does not correspond to a date format, we need to escape that. 

In this case, each and every character on that string has to be escaped as shown below.

The code.



```
<?php
$paragraph = &quot;today is 2014 January 1&quot;;
$date = DateTime::createFromFormat(&apos;\t\o\d\a\y \i\s Y F j&apos;, $paragraph);
echo $date-&gt;format(&apos;Y F&apos;); //&quot;prints&quot; 2014 January

- Shankar Damodaran


  

#



createFromFormat(&apos;U&apos;) has a strange behaviour: it ignores the datetimezone and the resulting DateTime object will always have GMT+0000 timezone.



```
<?php

$dt = DateTime::createFromFormat(&apos;U&apos;, time(), new DateTimeZone(&apos;CET&apos;));
var_dump($dt-&gt;format(&apos;Y-m-d H:i:s (T)&apos;), date(&apos;Y-m-d H:i:s (T)&apos;, time()));

?>
```


The problem is microtime() and time() returning the timestamp in current timezone.&#xA0; Instead of using time you can use &apos;now&apos; but to get a DateTimeObject with microseconds you have to write it this way to be sure to get the correct datetime:



```
<?php

$dt = \DateTime::createFromFormat(&apos;U.u&apos;, microtime(true))-&gt;setTimezone(new \DateTimeZone(date(&apos;T&apos;)));

?>
```



  

#



Reportedly, microtime() may return a timestamp number without a fractional part if the microseconds are exactly zero.&#xA0; I.e., &quot;1463772747&quot; instead of the expected &quot;1463772747.000000&quot;.&#xA0; number_format() can create a correct string representation of the microsecond timestamp every time, which can be useful for creating DateTime objects when used with DateTime::createFromFormat():



```
<?php
$now = DateTime::createFromFormat(&apos;U.u&apos;, number_format(microtime(true), 6, &apos;.&apos;, &apos;&apos;));
var_dump($now-&gt;format(&apos;Y-m-d H:i:s.u&apos;)); // E.g., string(26) &quot;2016-05-20 19:36:26.900794&quot;


  

#



If you&apos;re here because you&apos;re trying to create a date from a week number, you want to be using setISODate, as I discovered here:

http://www.lornajane.net/posts/2011/getting-dates-from-week-numbers-in-php

  

#



It can be confusing creating new DateTime from timestamp when your default timezone (date.timezone) is different from UTC and you are used to date()-function.

date()-function automatically uses your current timezone setting but DateTime::createFromFormat (or DateTime constructor) does not (it ignores tz-parameter).

You can get same results as date() by setting the timezone after object creation.



```
<?php
$ts = 1414706400;
$date1 = date(&quot;Y-m-d H:i&quot;, $ts);
$date2 = DateTime::createFromFormat(&quot;U&quot;, $ts)-&gt;setTimeZone(new DateTimeZone(date_default_timezone_get()))-&gt;format(&quot;Y-m-d H:i&quot;);
//$date1===$date2
?>
```



  

#



It seems that a pipe (&apos;|&apos;) option in formating string works only with PHP version 5.3.7 and newer. We had an issue with it on versions 5.3.2, 5.3.3, 5.3.6. Yet it was fine with 5.3.8 and 5.3.10.



By short example:



```
<?php

$timezone = new DateTimeZone(&apos;UTC&apos;);

$dateTime = DateTime::createFromFormat(&apos;dmY|&apos;, &apos;01011972&apos;, $timezone);

//$dateTime is FALSE in PHP v &lt;5.3.8

?>
```




Instead we used a workaround:



```
<?php

$dateTime = DateTime::createFromFormat(&apos;dmY&apos;, &apos;01011972&apos;, $timezone);

$dateTime-&gt;format(&apos;Y-m-d 00:00:00&apos;);

?>
```


which works fine.



====



Modified by admin to correct for version (5.3.7 not 5.3.8)

  

#



I&apos;ve found that on PHP 5.5.13 (not sure if it happens on other versions) if you enter a month larger than 12 on a format that takes numeric months, the result will be a DateTime object with its month equal to the number modulo 12 instead of returning false.



```
<?php
var_dump(DateTime::createFromFormat(&apos;Y-m-d&apos;, &apos;2013-22-01&apos;));
?>
```


results in:
class DateTime#4 (3) {
&#xA0; public $date =&gt;
&#xA0; string(19) &quot;2014-10-01 13:05:05&quot;
&#xA0; public $timezone_type =&gt;
&#xA0; int(3)
&#xA0; public $timezone =&gt;
&#xA0; string(3) &quot;UTC&quot;
}

  

#



Not a bug, but a strange issue today 2012-08-30 :



```
<?php
$date = &quot;2011-02&quot;;
echo $date.&quot;\n&quot;;
$d = DateTime::createFromFormat(&quot;Y-m&quot;,$date);
echo $d-&gt;format(&quot;Y-m&quot;);
?>
```

will display : 
2011-02
2011-03

It&apos;s because there is no 2011-02-30, so datetime will take march insteed of february ...

To fix it :


```
<?php 
$date = &quot;2011-02&quot;;
echo $date.&quot;\n&quot;;
$d = DateTime::createFromFormat(&quot;Y-m-d&quot;,$date.&quot;-01&quot;);
echo $d-&gt;format(&quot;Y-m&quot;);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/datetime.createfromformat.php)

**[To root](/README.md)**