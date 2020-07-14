# date



Things to be aware of when using week numbers with years.<br><br>

```
<?php
echo date("YW", strtotime("2011-01-07")); // gives 201101
echo date("YW", strtotime("2011-12-31")); // gives 201152
echo date("YW", strtotime("2011-01-01")); // gives 201152 too
?>
```


BUT



```
<?php
echo date("oW", strtotime("2011-01-07")); // gives 201101
echo date("oW", strtotime("2011-12-31")); // gives 201152
echo date("oW", strtotime("2011-01-01")); // gives 201052 (Year is different than previous example)
?>
```
<br><br>Reason:<br>Y is year from the date<br>o is ISO-8601 year number<br>W is ISO-8601 week number of year<br><br>Conclusion:<br>if using &apos;W&apos; for the week number use &apos;o&apos; for the year.  

#

For Microseconds, we can get by following:<br><br>echo date(&apos;Ymd His&apos;.substr((string)microtime(), 1, 8).&apos; e&apos;);<br><br>Thought, it might be useful to someone !  

#

FYI: there&apos;s a list of constants with predefined formats on the DateTime object, for example instead of outputting ISO 8601 dates with:<br><br>

```
<?php
echo date(&apos;c&apos;);
?>
```


or



```
<?php
echo date(&apos;Y-m-d\TH:i:sO&apos;);
?>
```


You can use



```
<?php
echo date(DateTime::ISO8601);
?>
```
<br><br>instead, which is much easier to read.  

#

this how you make an HTML5 &lt;time&gt; tag correctly<br><br>

```
<?php

echo &apos;&lt;time datetime="&apos;.date(&apos;c&apos;).&apos;"&gt;&apos;.date(&apos;Y - m - d&apos;).&apos;&lt;/time&gt;&apos;;

?>
```
<br><br>in the "datetime" attribute you should put a machine-readable value which represent time , the best value is a full time/date with ISO 8601 ( date(&apos;c&apos;) ) ,,, the attr will be hidden from users<br><br>and it doesn&apos;t really matter what you put as a shown value to the user,, any date/time format is okay !<br><br>This is very good for SEO especially search engines like Google .  

#

It&apos;s common for us to overthink the complexity of date/time calculations and underthink the power and flexibility of PHP&apos;s built-in functions.  Consider http://php.net/manual/en/function.date.php#108613<br><br>

```
<?php<br>function get_time_string($seconds)<br>{<br>    return date(&apos;H:i:s&apos;, strtotime("2000-01-01 + $seconds SECONDS"));<br>}  

#

If you have a problem with the different time zone, this is the solution for that.<br>

```
<?php
// first line of PHP
$defaultTimeZone=&apos;UTC&apos;;
if(date_default_timezone_get()!=$defaultTimeZone)) date_default_timezone_set($defaultTimeZone);

// somewhere in the code
function _date($format="r", $timestamp=false, $timezone=false)
{
    $userTimezone = new DateTimeZone(!empty($timezone) ? $timezone : &apos;GMT&apos;);
    $gmtTimezone = new DateTimeZone(&apos;GMT&apos;);
    $myDateTime = new DateTime(($timestamp!=false?date("r",(int)$timestamp):date("r")), $gmtTimezone);
    $offset = $userTimezone-&gt;getOffset($myDateTime);
    return date($format, ($timestamp!=false?(int)$timestamp:$myDateTime-&gt;format(&apos;U&apos;)) + $offset);
}

/* Example */
echo &apos;System Date/Time: &apos;.date("Y-m-d | h:i:sa").&apos;&lt;br&gt;&apos;;
echo &apos;New York Date/Time: &apos;._date("Y-m-d | h:i:sa", false, &apos;America/New_York&apos;).&apos;&lt;br&gt;&apos;;
echo &apos;Belgrade Date/Time: &apos;._date("Y-m-d | h:i:sa", false, &apos;Europe/Belgrade&apos;).&apos;&lt;br&gt;&apos;;
echo &apos;Belgrade Date/Time: &apos;._date("Y-m-d | h:i:sa", 514640700, &apos;Europe/Belgrade&apos;).&apos;&lt;br&gt;&apos;;
?>
```
<br>This is the best and fastest solution for this problem. Working almost identical to date() function only as a supplement has the time zone option.  

#

[Official documentation page](https://www.php.net/manual/en/function.date.php)

**[To root](/README.md)**