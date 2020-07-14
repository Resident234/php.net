# The DateInterval class



IRT note below: from PHP 7.0+, split seconds get returned, too, and will be under the `f` property.  

#

DateInterval does not support split seconds (microseconds or milliseconds etc.) when doing a diff between two DateTime objects that contain microseconds.<br><br>So you cannot do the following, for example:<br><br>

```
<?php
$d1=new DateTime("2012-07-08 11:14:15.638276");
$d2=new DateTime("2012-07-08 11:14:15.889342");
$diff=$d2-&gt;diff($d1);
print_r( $diff ) ;

/* returns:

DateInterval Object
(
    [y] =&gt; 0
    [m] =&gt; 0
    [d] =&gt; 0
    [h] =&gt; 0
    [i] =&gt; 0
    [s] =&gt; 0
    [invert] =&gt; 0
    [days] =&gt; 0
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
$diff=$d2-&gt;diff($d1);
print_r( $diff ) ;
?>
```


and it now DOES return microsecond differences, in the key &apos;f&apos;:



```
<?php
DateInterval Object
(
    [y] =&gt; 0
    [m] =&gt; 0
    [d] =&gt; 0
    [h] =&gt; 0
    [i] =&gt; 0
    [s] =&gt; 0
    [f] =&gt; 0.251066
    [weekday] =&gt; 0
    [weekday_behavior] =&gt; 0
    [first_last_day_of] =&gt; 0
    [invert] =&gt; 1
    [days] =&gt; 0
    [special_type] =&gt; 0
    [special_amount] =&gt; 0
    [have_weekday_relative] =&gt; 0
    [have_special_relative] =&gt; 0
)
?>
```
  

#

If you want to convert a Timespan given in Seconds into an DateInterval Object you could dot the following:<br><br>

```
<?php

$dv = new DateInterval(&apos;PT&apos;.$timespan.&apos;S&apos;);
?>
```


but wenn you look at the object, only the $dv-&gt;s property is set. 

As stated in the documentation to DateInterval::format

The DateInterval::format() method does not recalculate carry over points in time strings nor in date segments. This is expected because it is not possible to overflow values like "32 days" which could be interpreted as anything from "1 month and 4 days" to "1 month and 1 day". 

If you still want to calculate the seconds into hours / days / years, etc do the following:



```
<?php

$d1 = new DateTime();
$d2 = new DateTime();
$d2-&gt;add(new DateInterval(&apos;PT&apos;.$timespan.&apos;S&apos;));
      
$iv = $d2-&gt;diff($d1);

?>
```
<br><br>$iv is an DateInterval set with days, years, hours, seconds, etc ...  

#

[Official documentation page](https://www.php.net/manual/en/class.dateinterval.php)

**[To root](/README.md)**