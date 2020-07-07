# DateTime::__construct





There&apos;s a reason for ignoring the time zone when you pass a timestamp to __construct.&#xA0; That is, UNIX timestamps are by definition based on UTC.&#xA0; @1234567890 represents the same date/time regardless of time zone.&#xA0; So there&apos;s no need for a time zone at all.

  

#



The theoretical limits of the date range seem to be &quot;-9999-01-01&quot; through &quot;9999-12-31&quot; (PHP 5.2.9 on Windows Vista 64):



```
<?php

$d = new DateTime(&quot;9999-12-31&quot;); 
$d-&gt;format(&quot;Y-m-d&quot;); // &quot;9999-12-31&quot;

$d = new DateTime(&quot;0000-12-31&quot;); 
$d-&gt;format(&quot;Y-m-d&quot;); // &quot;0000-12-31&quot;

$d = new DateTime(&quot;-9999-12-31&quot;); 
$d-&gt;format(&quot;Y-m-d&quot;); // &quot;-9999-12-31&quot;

?>
```


Dates above 10000 and below -10000 do not throw errors but produce weird results:



```
<?php

$d = new DateTime(&quot;10019-01-01&quot;); 
$d-&gt;format(&quot;Y-m-d&quot;); // &quot;2009-01-01&quot;

$d = new DateTime(&quot;10009-01-01&quot;); 
$d-&gt;format(&quot;Y-m-d&quot;); // &quot;2009-01-01&quot;

$d = new DateTime(&quot;-10019-01-01&quot;); 
$d-&gt;format(&quot;Y-m-d&quot;); // &quot;2009-01-01&quot;

?>
```



  

#



A definite &quot;gotcha&quot; (while documented) that exists in the __construct is that it ignores your timezone if the $time is a timestamp.&#xA0; While this may not make sense, the object does provide you with methods to work around it.





```
<?php

// New Timezone Object

$timezone = new DateTimeZone(&apos;America/New_York&apos;);



// New DateTime Object

$date =&#xA0; new DateTime(&apos;@1306123200&apos;, $timezone);&#xA0; &#xA0; 



// You would expect the date to be 2011-05-23 00:00:00

// But it actually outputs 2011-05-23 04:00:00

echo $date-&gt;format(&apos;Y-m-d H:i:s&apos;);



// You can still set the timezone though like so...&#xA0; &#xA0; &#xA0; &#xA0; 

$date-&gt;setTimezone($timezone);



// This will now output 2011-05-23 00:00:00

echo $date-&gt;format(&apos;Y-m-d H:i:s&apos;);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/datetime.construct.php)

**[To root](/README.md)**