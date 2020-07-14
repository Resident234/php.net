# DateTime::diff



It is worth noting, IMO, and it is implied in the docs but not explicitly stated, that the object on which diff is called is subtracted from the object that is passed to diff.<br><br>i.e. $now-&gt;diff($tomorrow) is positive.  

#

Be careful using:<br><br>$date1 = new DateTime(&apos;now&apos;);<br>$date2 = new DateTime(&apos;tomorrow&apos;);<br><br>$interval = date_diff($date1, $date2);<br><br>echo $interval-&gt;format(&apos;In %a days&apos;);<br><br>In some situations, this won&apos;t say "in 1 days", but "in 0 days".<br>I think this is because "now" is the current time, while "tomorrow" is the current day +1 but at a default time, lets say:<br><br>Now: 08:00pm, 01.01.2015<br>Tomorrow: 00:00am, 02.01.2015<br><br>In this case, the difference is not 24 hour, so it will says 0 days.<br><br>Better use "today", which should also use a default value like:<br><br>Today: 00:00am, 01.01.2015<br>Tomorrow: 00:00am, 02.01.2015<br><br>which now is 24 hour and represents 1 day.<br><br>This may sound logical and many will say "of course, this is right", but if you use it in a naiv way (like I did without thinking), you can come to this moment and facepalm yourself.<br><br>Conclusion: "Now" is "Today", but in a different clock time, but still the same day!  

#

After wrestling with DateTime::diff for a while it finally dawned on me the problem was both in the formatting of the input string and the formatting of the output.<br><br>The task was to calculate the duration between two date/times.<br><br>### Calculating Duration <br><br>1. Make sure you have a valid date variable.  Both of these strings are valid:<br><br>

```
<?php

// Example

   $strStart = &apos;2013-06-19 18:25&apos;;
   $strEnd   = &apos;06/19/13 21:47&apos;;

?>
```


2. Next convert the string to a date variable
~~~


```
<?php

   $dteStart = new DateTime($strStart);
   $dteEnd   = new DateTime($strEnd);

?>
```

~~~

3. Calculate the difference
~~~


```
<?php

   $dteDiff  = $dteStart-&gt;diff($dteEnd);

?>
```

~~~

4. Format the output
~~~


```
<?php

   print $dteDiff-&gt;format("%H:%I:%S");

/*
    Outputs
    
    03:22:00
*/

?>
```
<br>~~~<br><br>[Modified by moderator for clarify]  

#

Using the identical (===) comparision operator in different but equal objects will return false<br><br>

```
<?php
$c = new DateTime( &apos;2014-04-20&apos; );
$d = new DateTime( &apos;2014-04-20&apos; );
var_dump( $d === $d ); #true
var_dump( $d === $c ); #false
var_dump( $d == $c ); #true
?>
```
  

#

If you want to quickly scan through the resulting intervals, you can use the undocumented properties of DateInterval.<br><br>The function below returns a single number of years, months, days, hours, minutes or seconds between the current date and the provided date.  If the date occurs in the past (is negative/inverted), it suffixes it with &apos;ago&apos;.<br><br>

```
<?php
function pluralize( $count, $text ) 
{ 
    return $count . ( ( $count == 1 ) ? ( " $text" ) : ( " ${text}s" ) );
}

function ago( $datetime )
{
    $interval = date_create(&apos;now&apos;)-&gt;diff( $datetime );
    $suffix = ( $interval-&gt;invert ? &apos; ago&apos; : &apos;&apos; );
    if ( $v = $interval-&gt;y &gt;= 1 ) return pluralize( $interval-&gt;y, &apos;year&apos; ) . $suffix;
    if ( $v = $interval-&gt;m &gt;= 1 ) return pluralize( $interval-&gt;m, &apos;month&apos; ) . $suffix;
    if ( $v = $interval-&gt;d &gt;= 1 ) return pluralize( $interval-&gt;d, &apos;day&apos; ) . $suffix;
    if ( $v = $interval-&gt;h &gt;= 1 ) return pluralize( $interval-&gt;h, &apos;hour&apos; ) . $suffix;
    if ( $v = $interval-&gt;i &gt;= 1 ) return pluralize( $interval-&gt;i, &apos;minute&apos; ) . $suffix;
    return pluralize( $interval-&gt;s, &apos;second&apos; ) . $suffix;
}
?>
```
  

#

It seems that while DateTime in general does preserve microseconds, DateTime::diff doesn&apos;t appear to account for it when comparing.  <br><br>Example:<br><br>

```
<?php
$val1 = &apos;2014-03-18 10:34:09.939&apos;;
$val2 = &apos;2014-03-18 10:34:09.940&apos;;

$datetime1 = new DateTime($val1);
$datetime2 = new DateTime($val2);
echo "&lt;pre&gt;";
var_dump($datetime1-&gt;diff($datetime2));

if($datetime1 &gt; $datetime2)
  echo "1 is bigger";
else
  echo "2 is bigger";
?>
```


The var_dump shows that there is no "u" element, and "2 is bigger" is echoed. 

To work around this apparent limitation/oversight, you have to additionally compare using DateTime::format.  

Example:



```
<?php
if($datetime1 &gt; $datetime2)
  echo "1 is bigger";
else if ($datetime1-&gt;format(&apos;u&apos;) &gt; $datetime2-&gt;format(&apos;u&apos;))
  echo "1 is bigger";
else
  echo "2 is bigger";
?>
```
  

#

Warning, there&apos;s a bug on windows platforms: the result is always 6015 days (and not 42...)<br><br>http://bugs.php.net/bug.php?id=51184  

#

Though I found a number of people who ran into the issue of 5.2 and lower not supporting this function, I was unable to find any solid examples to get around it. Therefore I hope this can help some others:<br><br>

```
<?php
function get_timespan_string($older, $newer) {
  $Y1 = $older-&gt;format(&apos;Y&apos;);
  $Y2 = $newer-&gt;format(&apos;Y&apos;);
  $Y = $Y2 - $Y1;

  $m1 = $older-&gt;format(&apos;m&apos;);
  $m2 = $newer-&gt;format(&apos;m&apos;);
  $m = $m2 - $m1;

  $d1 = $older-&gt;format(&apos;d&apos;);
  $d2 = $newer-&gt;format(&apos;d&apos;);
  $d = $d2 - $d1;

  $H1 = $older-&gt;format(&apos;H&apos;);
  $H2 = $newer-&gt;format(&apos;H&apos;);
  $H = $H2 - $H1;

  $i1 = $older-&gt;format(&apos;i&apos;);
  $i2 = $newer-&gt;format(&apos;i&apos;);
  $i = $i2 - $i1;

  $s1 = $older-&gt;format(&apos;s&apos;);
  $s2 = $newer-&gt;format(&apos;s&apos;);
  $s = $s2 - $s1;

  if($s &lt; 0) {
    $i = $i -1;
    $s = $s + 60;
  }
  if($i &lt; 0) {
    $H = $H - 1;
    $i = $i + 60;
  }
  if($H &lt; 0) {
    $d = $d - 1;
    $H = $H + 24;
  }
  if($d &lt; 0) {
    $m = $m - 1;
    $d = $d + get_days_for_previous_month($m2, $Y2);
  }
  if($m &lt; 0) {
    $Y = $Y - 1;
    $m = $m + 12;
  }
  $timespan_string = create_timespan_string($Y, $m, $d, $H, $i, $s);
  return $timespan_string;
}

function get_days_for_previous_month($current_month, $current_year) {
  $previous_month = $current_month - 1;
  if($current_month == 1) {
    $current_year = $current_year - 1; //going from January to previous December
    $previous_month = 12;
  }
  if($previous_month == 11 || $previous_month == 9 || $previous_month == 6 || $previous_month == 4) {
    return 30;
  }
  else if($previous_month == 2) {
    if(($current_year % 4) == 0) { //remainder 0 for leap years
      return 29;
    }
    else {
      return 28;
    }
  }
  else {
    return 31;
  }
}

function create_timespan_string($Y, $m, $d, $H, $i, $s)
{
  $timespan_string = &apos;&apos;;
  $found_first_diff = false;
  if($Y &gt;= 1) {
    $found_first_diff = true;
    $timespan_string .= pluralize($Y, &apos;year&apos;).&apos; &apos;;
  }
  if($m &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($m, &apos;month&apos;).&apos; &apos;;
  }
  if($d &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($d, &apos;day&apos;).&apos; &apos;;
  }
  if($H &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($H, &apos;hour&apos;).&apos; &apos;;
  }
  if($i &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($i, &apos;minute&apos;).&apos; &apos;;
  }
  if($found_first_diff) {
    $timespan_string .= &apos;and &apos;;
  }
  $timespan_string .= pluralize($s, &apos;second&apos;);
  return $timespan_string;
}

function pluralize( $count, $text )
{
  return $count . ( ( $count == 1 ) ? ( " $text" ) : ( " ${text}s" ) );
}
?>
```
  

#

I needed to get the exact number of days between 2 dates and was relying on the this diff function, but found that I was getting a peculiar result with:<br><br>

```
<?php
    $today = new DateTime(date(&apos;2011-11-09&apos;));
    $appt  = new DateTime(date(&apos;2011-12-09&apos;));
    $days_until_appt = $appt-&gt;diff($today)-&gt;d;
?>
```


This was returning 0 because it was exactly one month.

I had to end up using :



```
<?php
    $days_until_appt = $appt-&gt;diff($today)-&gt;days;
?>
```
<br><br>to get 30.  

#

[Official documentation page](https://www.php.net/manual/en/datetime.diff.php)

**[To root](/README.md)**