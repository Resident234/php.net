# The DateInterval class



IRT note below: from PHP 7.0+, split seconds get returned, too, and will be under the `f` property.  

#

DateInterval does not support split seconds (microseconds or milliseconds etc.) when doing a diff between two DateTime objects that contain microseconds.<br><br>So you cannot do the following, for example:<br><br>

```
<?php
$d1=new DateTime("2012-07-08 11:14:15.638276");
$d2=new DateTime("2012-07-08 11:14:15.889342");
$diff=$d2->diff($d1);
print_r( $diff ) ;

/* returns:

DateInterval Object
(
    [y] => 0
    [m] => 0
    [d] => 0
    [h] => 0
    [i] => 0
    [s] => 0
    [invert] => 0
    [days] => 0
)

*/
?>
```
<br><br>You get back 0 when you actually want to get 0.251066 seconds.  

#

Just tried the code submitted earlier on php 7.2.16:<br><br>

```
<?php
$d1=new DateTime("2012-07-08 11:14:15.638276");
$d2=new DateTime("2012-07-08 11:14:15.889342");
$diff=$d2->diff($d1);
print_r( $diff ) ;
?>
```


and it now DOES return microsecond differences, in the key 'f':



```
<?php
DateInterval Object
(
    [y] => 0
    [m] => 0
    [d] => 0
    [h] => 0
    [i] => 0
    [s] => 0
    [f] => 0.251066
    [weekday] => 0
    [weekday_behavior] => 0
    [first_last_day_of] => 0
    [invert] => 1
    [days] => 0
    [special_type] => 0
    [special_amount] => 0
    [have_weekday_relative] => 0
    [have_special_relative] => 0
)
?>
```
  

#

If you want to convert a Timespan given in Seconds into an DateInterval Object you could dot the following:<br><br>

```
<?php

$dv = new DateInterval('PT'.$timespan.'S');
?>
```


but wenn you look at the object, only the $dv->s property is set. 

As stated in the documentation to DateInterval::format

The DateInterval::format() method does not recalculate carry over points in time strings nor in date segments. This is expected because it is not possible to overflow values like "32 days" which could be interpreted as anything from "1 month and 4 days" to "1 month and 1 day". 

If you still want to calculate the seconds into hours / days / years, etc do the following:



```
<?php

$d1 = new DateTime();
$d2 = new DateTime();
$d2->add(new DateInterval('PT'.$timespan.'S'));
      
$iv = $d2->diff($d1);

?>
```
<br><br>$iv is an DateInterval set with days, years, hours, seconds, etc ...  

#

[Official documentation page](https://www.php.net/manual/en/class.dateinterval.php)

**[To root](/README.md)**