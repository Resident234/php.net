# date





Things to be aware of when using week numbers with years.



```
<?php
echo date(&quot;YW&quot;, strtotime(&quot;2011-01-07&quot;)); // gives 201101
echo date(&quot;YW&quot;, strtotime(&quot;2011-12-31&quot;)); // gives 201152
echo date(&quot;YW&quot;, strtotime(&quot;2011-01-01&quot;)); // gives 201152 too
?>
```


BUT



```
<?php
echo date(&quot;oW&quot;, strtotime(&quot;2011-01-07&quot;)); // gives 201101
echo date(&quot;oW&quot;, strtotime(&quot;2011-12-31&quot;)); // gives 201152
echo date(&quot;oW&quot;, strtotime(&quot;2011-01-01&quot;)); // gives 201052 (Year is different than previous example)
?>
```


Reason:
Y is year from the date
o is ISO-8601 year number
W is ISO-8601 week number of year

Conclusion:
if using &apos;W&apos; for the week number use &apos;o&apos; for the year.

  

#



For Microseconds, we can get by following:

echo date(&apos;Ymd His&apos;.substr((string)microtime(), 1, 8).&apos; e&apos;);

Thought, it might be useful to someone !

  

#



FYI: there&apos;s a list of constants with predefined formats on the DateTime object, for example instead of outputting ISO 8601 dates with:



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


instead, which is much easier to read.

  

#



this how you make an HTML5 &lt;time&gt; tag correctly



```
<?php

echo &apos;&lt;time datetime=&quot;&apos;.date(&apos;c&apos;).&apos;&quot;&gt;&apos;.date(&apos;Y - m - d&apos;).&apos;&lt;/time&gt;&apos;;

?>
```


in the &quot;datetime&quot; attribute you should put a machine-readable value which represent time , the best value is a full time/date with ISO 8601 ( date(&apos;c&apos;) ) ,,, the attr will be hidden from users

and it doesn&apos;t really matter what you put as a shown value to the user,, any date/time format is okay !

This is very good for SEO especially search engines like Google .

  

#



It&apos;s common for us to overthink the complexity of date/time calculations and underthink the power and flexibility of PHP&apos;s built-in functions.&#xA0; Consider http://php.net/manual/en/function.date.php#108613



```
<?php
function get_time_string($seconds)
{
&#xA0; &#xA0; return date(&apos;H:i:s&apos;, strtotime(&quot;2000-01-01 + $seconds SECONDS&quot;));
}


  

#



If you have a problem with the different time zone, this is the solution for that.


```
<?php
// first line of PHP
$defaultTimeZone=&apos;UTC&apos;;
if(date_default_timezone_get()!=$defaultTimeZone)) date_default_timezone_set($defaultTimeZone);

// somewhere in the code
function _date($format=&quot;r&quot;, $timestamp=false, $timezone=false)
{
&#xA0; &#xA0; $userTimezone = new DateTimeZone(!empty($timezone) ? $timezone : &apos;GMT&apos;);
&#xA0; &#xA0; $gmtTimezone = new DateTimeZone(&apos;GMT&apos;);
&#xA0; &#xA0; $myDateTime = new DateTime(($timestamp!=false?date(&quot;r&quot;,(int)$timestamp):date(&quot;r&quot;)), $gmtTimezone);
&#xA0; &#xA0; $offset = $userTimezone-&gt;getOffset($myDateTime);
&#xA0; &#xA0; return date($format, ($timestamp!=false?(int)$timestamp:$myDateTime-&gt;format(&apos;U&apos;)) + $offset);
}

/* Example */
echo &apos;System Date/Time: &apos;.date(&quot;Y-m-d | h:i:sa&quot;).&apos;&lt;br&gt;&apos;;
echo &apos;New York Date/Time: &apos;._date(&quot;Y-m-d | h:i:sa&quot;, false, &apos;America/New_York&apos;).&apos;&lt;br&gt;&apos;;
echo &apos;Belgrade Date/Time: &apos;._date(&quot;Y-m-d | h:i:sa&quot;, false, &apos;Europe/Belgrade&apos;).&apos;&lt;br&gt;&apos;;
echo &apos;Belgrade Date/Time: &apos;._date(&quot;Y-m-d | h:i:sa&quot;, 514640700, &apos;Europe/Belgrade&apos;).&apos;&lt;br&gt;&apos;;
?>
```

This is the best and fastest solution for this problem. Working almost identical to date() function only as a supplement has the time zone option.

  

#

[Official documentation page](https://www.php.net/manual/en/function.date.php)

**[To root](/README.md)**