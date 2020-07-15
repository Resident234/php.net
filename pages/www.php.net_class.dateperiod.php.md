# The DatePeriod class



Just an example to include the end date using the DateTime method &apos;modify&apos;<br><br>

```
<?php

$begin = new DateTime( '2012-08-01' );
$end = new DateTime( '2012-08-31' );
$end = $end->modify( '+1 day' ); 

$interval = new DateInterval('P1D');
$daterange = new DatePeriod($begin, $interval ,$end);

foreach($daterange as $date){
    echo $date->format("Ymd") . "&lt;br&gt;";
}
?>
```
  

#

Thanks much to those of you who supplied sample code; that helps a lot.<br><br>I wanted to mention another thing that helped me: when you do that foreach ( $period as $dt ), the $dt values are DateTime objects.<br><br>That may be obvious to those of you with more experience, but I wasn&apos;t sure until I looked it up on Stack Overflow. So I figured it was worth posting here to help others like me who might&apos;ve been confused or uncertain.  

#

Nice example from PHP Spring Conference (thanks to Johannes Schl&#xFC;ter and David Z&#xFC;lke)<br><br>

```
<?php
$begin = new DateTime( '2007-12-31' );
$end = new DateTime( '2009-12-31 23:59:59' );

$interval = DateInterval::createFromDateString('last thursday of next month');
$period = new DatePeriod($begin, $interval, $end, DatePeriod::EXCLUDE_START_DATE);

foreach ( $period as $dt )
  echo $dt->format( "l Y-m-d H:i:s\n" );
?>
```
<br><br>DateInterval specs could be found at http://en.wikipedia.org/wiki/ISO_8601#Time_intervals  

#

[Official documentation page](https://www.php.net/manual/en/class.dateperiod.php)

**[To root](/README.md)**