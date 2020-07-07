# DatePeriod::__construct





I found two things useful to know that aren&apos;t covered here.



1. endDate is excluded:





```
<?php

$i = new DateInterval(&apos;P1D&apos;);

$d1 = new Datetime();

$d2 = clone $d1; $d2-&gt;add($i);

foreach(new DatePeriod($d1, $i, $d2) as $d) {

&#xA0; &#xA0; echo $d-&gt;format(&apos;Y-m-d H:i:s&apos;) . &quot;\n&quot;;

}

?>
```




Will output:

2010-11-03 12:39:53



(Another one because I got it wrong at first)

2. For the first form, recurrences really means REcurrences, not occurences.





```
<?php

$i = new DateInterval(&apos;P1D&apos;);

$d = new Datetime();

foreach(new DatePeriod($d, $i, 1) as $d) {

&#xA0; &#xA0; echo $d-&gt;format(&apos;Y-m-d H:i:s&apos;) . &quot;\n&quot;;

}

?>
```




Will output:

2010-11-03 12:41:05

2010-11-04 12:41:05

  

#



When you add the time 23:59:59 to the end DateTime object something like the following then the end date will be included in the period:





```
<?php

$date_start = new DateTime(&apos;2012-03-12&apos;);

$date_end = new DateTime(&apos;2012-03-22 23:59:59&apos;);



$interval = &apos;+2 days&apos;;

$date_interval = DateInterval::createFromDateString($interval);



$period = new DatePeriod($date_start, $date_interval, $date_end, DatePeriod::EXCLUDE_START_DATE);



foreach($period as $dt) {

 echo $dt-&gt;format(&apos;d/m&apos;);

}

?>
```




OUTPUT:

14/03

16/03

18/03

20/03

22/03

  

#

[Official documentation page](https://www.php.net/manual/en/dateperiod.construct.php)

**[To root](/README.md)**