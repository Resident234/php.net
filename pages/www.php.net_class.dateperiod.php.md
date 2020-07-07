# The DatePeriod class





Just an example to include the end date using the DateTime method &apos;modify&apos;



```
<?php

$begin = new DateTime( &apos;2012-08-01&apos; );
$end = new DateTime( &apos;2012-08-31&apos; );
$end = $end-&gt;modify( &apos;+1 day&apos; ); 

$interval = new DateInterval(&apos;P1D&apos;);
$daterange = new DatePeriod($begin, $interval ,$end);

foreach($daterange as $date){
&#xA0; &#xA0; echo $date-&gt;format(&quot;Ymd&quot;) . &quot;&lt;br&gt;&quot;;
}
?>
```



  

#



Thanks much to those of you who supplied sample code; that helps a lot.

I wanted to mention another thing that helped me: when you do that foreach ( $period as $dt ), the $dt values are DateTime objects.

That may be obvious to those of you with more experience, but I wasn&apos;t sure until I looked it up on Stack Overflow. So I figured it was worth posting here to help others like me who might&apos;ve been confused or uncertain.

  

#



Nice example from PHP Spring Conference (thanks to Johannes Schl&#xFC;ter and David Z&#xFC;lke)



```
<?php
$begin = new DateTime( &apos;2007-12-31&apos; );
$end = new DateTime( &apos;2009-12-31 23:59:59&apos; );

$interval = DateInterval::createFromDateString(&apos;last thursday of next month&apos;);
$period = new DatePeriod($begin, $interval, $end, DatePeriod::EXCLUDE_START_DATE);

foreach ( $period as $dt )
&#xA0; echo $dt-&gt;format( &quot;l Y-m-d H:i:s\n&quot; );
?>
```


DateInterval specs could be found at http://en.wikipedia.org/wiki/ISO_8601#Time_intervals

  

#

[Official documentation page](https://www.php.net/manual/en/class.dateperiod.php)

**[To root](/README.md)**