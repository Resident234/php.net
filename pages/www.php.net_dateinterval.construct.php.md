# DateInterval::__construct





M is used to indicate both months and minutes.

As noted on the referenced wikipedia page for ISO 6801 http://en.wikipedia.org/wiki/Iso8601#Durations

To resolve ambiguity, &quot;P1M&quot; is a one-month duration and &quot;PT1M&quot; is a one-minute duration (note the time designator, T, that precedes the time value).

Using: PHP 5.3.2-1ubuntu4.19

// For 3 Months
$dateTime = new DateTime;echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;
$dateTime-&gt;add(new DateInterval(&quot;P3M&quot;));
echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;
Results in:
2013-07-11T11:12:26-0400
2013-10-11T11:12:26-0400

// For 3 Minutes
$dateTime = new DateTime;echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;
$dateTime-&gt;add(new DateInterval(&quot;PT3M&quot;));
echo $dateTime-&gt;format( DateTime::ISO8601 ), PHP_EOL;
Results in:
2013-07-11T11:12:42-0400
2013-07-11T11:15:42-0400

Insert a T after the P in the interval to add 3 minutes instead of 3 months.

  

#



I think it is easiest if you would just use the sub method on the DateTime class.



```
<?php
$date = new DateTime();
$date-&gt;sub(new DateInterval(&quot;P89D&quot;));


  

#



Note that, while a DateInterval object has an $invert property, you cannot supply a negative directly to the constructor similar to specifying a negative in XSD (&quot;-P1Y&quot;). You will get an exception through if you do this. 

Instead you need to construct using a positive interval (&quot;P1Y&quot;) and the specify the $invert property === 1.

  

#



It should be noted that this class will not calculate days/hours/minutes/seconds etc given a value in a single denomination of time.&#xA0; For example:



```
<?php
&#xA0; &#xA0; $di = new DateInterval(&apos;PT3600S&apos;);
&#xA0; &#xA0; echo $di-&gt;format(&apos;%H:%i:%s&apos;);
&#xA0; &#xA0; 
?>
```


will yield 0:0:3600 instead of the expected 1:0:0

  

#

[Official documentation page](https://www.php.net/manual/en/dateinterval.construct.php)

**[To root](/README.md)**