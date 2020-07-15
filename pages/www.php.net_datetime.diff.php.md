# DateTime::diff



It is worth noting, IMO, and it is implied in the docs but not explicitly stated, that the object on which diff is called is subtracted from the object that is passed to diff.<br><br>i.e. $now-&gt;diff($tomorrow) is positive.  

#

Be careful using:<br><br>$date1 = new DateTime(&apos;now&apos;);<br>$date2 = new DateTime(&apos;tomorrow&apos;);<br><br>$interval = date_diff($date1, $date2);<br><br>echo $interval-&gt;format(&apos;In %a days&apos;);<br><br>In some situations, this won&apos;t say "in 1 days", but "in 0 days".<br>I think this is because "now" is the current time, while "tomorrow" is the current day +1 but at a default time, lets say:<br><br>Now: 08:00pm, 01.01.2015<br>Tomorrow: 00:00am, 02.01.2015<br><br>In this case, the difference is not 24 hour, so it will says 0 days.<br><br>Better use "today", which should also use a default value like:<br><br>Today: 00:00am, 01.01.2015<br>Tomorrow: 00:00am, 02.01.2015<br><br>which now is 24 hour and represents 1 day.<br><br>This may sound logical and many will say "of course, this is right", but if you use it in a naiv way (like I did without thinking), you can come to this moment and facepalm yourself.<br><br>Conclusion: "Now" is "Today", but in a different clock time, but still the same day!  

#

After wrestling with DateTime::diff for a while it finally dawned on me the problem was both in the formatting of the input string and the formatting of the output.<br><br>The task was to calculate the duration between two date/times.<br><br>### Calculating Duration <br><br>1. Make sure you have a valid date variable.  Both of these strings are valid:<br><br>

```
<?php

// Example

   $strStart = '2013-06-19 18:25';
   $strEnd   = '06/19/13 21:47';

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

   $dteDiff  = $dteStart->diff($dteEnd);

?>
```

~~~

4. Format the output
~~~


```
<?php

   print $dteDiff->format("%H:%I:%S");

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
$c = new DateTime( '2014-04-20' );
$d = new DateTime( '2014-04-20' );
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
    $interval = date_create('now')->diff( $datetime );
    $suffix = ( $interval->invert ? ' ago' : '' );
    if ( $v = $interval->y &gt;= 1 ) return pluralize( $interval->y, 'year' ) . $suffix;
    if ( $v = $interval->m &gt;= 1 ) return pluralize( $interval->m, 'month' ) . $suffix;
    if ( $v = $interval->d &gt;= 1 ) return pluralize( $interval->d, 'day' ) . $suffix;
    if ( $v = $interval->h &gt;= 1 ) return pluralize( $interval->h, 'hour' ) . $suffix;
    if ( $v = $interval->i &gt;= 1 ) return pluralize( $interval->i, 'minute' ) . $suffix;
    return pluralize( $interval->s, 'second' ) . $suffix;
}
?>
```
  

#

It seems that while DateTime in general does preserve microseconds, DateTime::diff doesn&apos;t appear to account for it when comparing.  <br><br>Example:<br><br>

```
<?php
$val1 = '2014-03-18 10:34:09.939';
$val2 = '2014-03-18 10:34:09.940';

$datetime1 = new DateTime($val1);
$datetime2 = new DateTime($val2);
echo "&lt;pre&gt;";
var_dump($datetime1->diff($datetime2));

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
else if ($datetime1->format('u') &gt; $datetime2->format('u'))
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
  $Y1 = $older->format('Y');
  $Y2 = $newer->format('Y');
  $Y = $Y2 - $Y1;

  $m1 = $older->format('m');
  $m2 = $newer->format('m');
  $m = $m2 - $m1;

  $d1 = $older->format('d');
  $d2 = $newer->format('d');
  $d = $d2 - $d1;

  $H1 = $older->format('H');
  $H2 = $newer->format('H');
  $H = $H2 - $H1;

  $i1 = $older->format('i');
  $i2 = $newer->format('i');
  $i = $i2 - $i1;

  $s1 = $older->format('s');
  $s2 = $newer->format('s');
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
  $timespan_string = '';
  $found_first_diff = false;
  if($Y &gt;= 1) {
    $found_first_diff = true;
    $timespan_string .= pluralize($Y, 'year').' ';
  }
  if($m &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($m, 'month').' ';
  }
  if($d &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($d, 'day').' ';
  }
  if($H &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($H, 'hour').' ';
  }
  if($i &gt;= 1 || $found_first_diff) {
    $found_first_diff = true;
    $timespan_string .= pluralize($i, 'minute').' ';
  }
  if($found_first_diff) {
    $timespan_string .= 'and ';
  }
  $timespan_string .= pluralize($s, 'second');
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
    $today = new DateTime(date('2011-11-09'));
    $appt  = new DateTime(date('2011-12-09'));
    $days_until_appt = $appt->diff($today)->d;
?>
```


This was returning 0 because it was exactly one month.

I had to end up using :



```
<?php
    $days_until_appt = $appt->diff($today)->days;
?>
```
<br><br>to get 30.  

#

[Official documentation page](https://www.php.net/manual/en/datetime.diff.php)

**[To root](/README.md)**