# DateTime::setTimezone





In response to the other comments expressing surprise that changing the timezone does not affect the timestamp:

A UNIX timestamp is defined as the number of seconds that have elapsed since 00:00:00 (UTC), Thursday, 1 January 1970.

So: with respect to UTC. Always.

Calling setTimezone() never changes the actual &quot;absolute&quot;, underlying, moment-in-time itself. It only changes the timezone you wish to &quot;view&quot; that moment &quot;from&quot;. Consider the following:



```
<?php
// A time in London.
$datetime = new DateTime(&apos;2015-06-22T10:40:25&apos;, new DateTimeZone(&apos;Europe/London&apos;));

// I wonder how that SAME moment-in-time would 
// be described in other places around the world.
$datetime-&gt;setTimezone(new DateTimeZone(&apos;Australia/Sydney&apos;));
print $datetime-&gt;format(&apos;Y-m-d H:i:s (e)&apos;);
&#xA0; // 2015-06-22 19:40:25 (Australia/Sydney)

$datetime-&gt;setTimezone(new DateTimeZone(&apos;America/New_York&apos;));
print $datetime-&gt;format(&apos;Y-m-d H:i:s (e)&apos;);
&#xA0; // 2015-06-22 05:40:25 (America/New_York)

$datetime-&gt;setTimezone(new DateTimeZone(&apos;Asia/Calcutta&apos;));
print $datetime-&gt;format(&apos;Y-m-d H:i:s (e)&apos;);
&#xA0; // 2015-06-22 15:10:25 (Asia/Calcutta)
?>
```


Please note that ALL of these date strings unambiguously represent the exact same moment-in-time. Therefore, calling getTimestamp() at any stage will return the same result:



```
<?php
$datetime-&gt;getTimestamp();
&#xA0; // 1434966025
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/datetime.settimezone.php)

**[To root](/README.md)**