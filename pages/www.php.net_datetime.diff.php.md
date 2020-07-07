# DateTime::diff





It is worth noting, IMO, and it is implied in the docs but not explicitly stated, that the object on which diff is called is subtracted from the object that is passed to diff.

i.e. $now-&gt;diff($tomorrow) is positive.

  

#



Be careful using:

$date1 = new DateTime(&apos;now&apos;);
$date2 = new DateTime(&apos;tomorrow&apos;);

$interval = date_diff($date1, $date2);

echo $interval-&gt;format(&apos;In %a days&apos;);

In some situations, this won&apos;t say &quot;in 1 days&quot;, but &quot;in 0 days&quot;.
I think this is because &quot;now&quot; is the current time, while &quot;tomorrow&quot; is the current day +1 but at a default time, lets say:

Now: 08:00pm, 01.01.2015
Tomorrow: 00:00am, 02.01.2015

In this case, the difference is not 24 hour, so it will says 0 days.

Better use &quot;today&quot;, which should also use a default value like:

Today: 00:00am, 01.01.2015
Tomorrow: 00:00am, 02.01.2015

which now is 24 hour and represents 1 day.

This may sound logical and many will say &quot;of course, this is right&quot;, but if you use it in a naiv way (like I did without thinking), you can come to this moment and facepalm yourself.

Conclusion: &quot;Now&quot; is &quot;Today&quot;, but in a different clock time, but still the same day!

  

#



After wrestling with DateTime::diff for a while it finally dawned on me the problem was both in the formatting of the input string and the formatting of the output.



The task was to calculate the duration between two date/times.



### Calculating Duration 



1. Make sure you have a valid date variable.&#xA0; Both of these strings are valid:





```
<?php



// Example



&#xA0;&#xA0; $strStart = &apos;2013-06-19 18:25&apos;;

&#xA0;&#xA0; $strEnd&#xA0;&#xA0; = &apos;06/19/13 21:47&apos;;



?>
```




2. Next convert the string to a date variable

~~~



```
<?php



&#xA0;&#xA0; $dteStart = new DateTime($strStart);

&#xA0;&#xA0; $dteEnd&#xA0;&#xA0; = new DateTime($strEnd);



?>
```


~~~



3. Calculate the difference

~~~



```
<?php



&#xA0;&#xA0; $dteDiff&#xA0; = $dteStart-&gt;diff($dteEnd);



?>
```


~~~



4. Format the output

~~~



```
<?php



&#xA0;&#xA0; print $dteDiff-&gt;format(&quot;%H:%I:%S&quot;);



/*

&#xA0; &#xA0; Outputs

&#xA0; &#xA0; 

&#xA0; &#xA0; 03:22:00

*/



?>
```


~~~



[Modified by moderator for clarify]

  

#



Using the identical (===) comparision operator in different but equal objects will return false



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



If you want to quickly scan through the resulting intervals, you can use the undocumented properties of DateInterval.

The function below returns a single number of years, months, days, hours, minutes or seconds between the current date and the provided date.&#xA0; If the date occurs in the past (is negative/inverted), it suffixes it with &apos;ago&apos;.



```
<?php
function pluralize( $count, $text ) 
{ 
&#xA0; &#xA0; return $count . ( ( $count == 1 ) ? ( &quot; $text&quot; ) : ( &quot; ${text}s&quot; ) );
}

function ago( $datetime )
{
&#xA0; &#xA0; $interval = date_create(&apos;now&apos;)-&gt;diff( $datetime );
&#xA0; &#xA0; $suffix = ( $interval-&gt;invert ? &apos; ago&apos; : &apos;&apos; );
&#xA0; &#xA0; if ( $v = $interval-&gt;y &gt;= 1 ) return pluralize( $interval-&gt;y, &apos;year&apos; ) . $suffix;
&#xA0; &#xA0; if ( $v = $interval-&gt;m &gt;= 1 ) return pluralize( $interval-&gt;m, &apos;month&apos; ) . $suffix;
&#xA0; &#xA0; if ( $v = $interval-&gt;d &gt;= 1 ) return pluralize( $interval-&gt;d, &apos;day&apos; ) . $suffix;
&#xA0; &#xA0; if ( $v = $interval-&gt;h &gt;= 1 ) return pluralize( $interval-&gt;h, &apos;hour&apos; ) . $suffix;
&#xA0; &#xA0; if ( $v = $interval-&gt;i &gt;= 1 ) return pluralize( $interval-&gt;i, &apos;minute&apos; ) . $suffix;
&#xA0; &#xA0; return pluralize( $interval-&gt;s, &apos;second&apos; ) . $suffix;
}
?>
```



  

#



It seems that while DateTime in general does preserve microseconds, DateTime::diff doesn&apos;t appear to account for it when comparing.&#xA0; 

Example:



```
<?php
$val1 = &apos;2014-03-18 10:34:09.939&apos;;
$val2 = &apos;2014-03-18 10:34:09.940&apos;;

$datetime1 = new DateTime($val1);
$datetime2 = new DateTime($val2);
echo &quot;&lt;pre&gt;&quot;;
var_dump($datetime1-&gt;diff($datetime2));

if($datetime1 &gt; $datetime2)
&#xA0; echo &quot;1 is bigger&quot;;
else
&#xA0; echo &quot;2 is bigger&quot;;
?>
```


The var_dump shows that there is no &quot;u&quot; element, and &quot;2 is bigger&quot; is echoed. 

To work around this apparent limitation/oversight, you have to additionally compare using DateTime::format.&#xA0; 

Example:



```
<?php
if($datetime1 &gt; $datetime2)
&#xA0; echo &quot;1 is bigger&quot;;
else if ($datetime1-&gt;format(&apos;u&apos;) &gt; $datetime2-&gt;format(&apos;u&apos;))
&#xA0; echo &quot;1 is bigger&quot;;
else
&#xA0; echo &quot;2 is bigger&quot;;
?>
```



  

#



Warning, there&apos;s a bug on windows platforms: the result is always 6015 days (and not 42...)

http://bugs.php.net/bug.php?id=51184

  

#



Though I found a number of people who ran into the issue of 5.2 and lower not supporting this function, I was unable to find any solid examples to get around it. Therefore I hope this can help some others:





```
<?php

function get_timespan_string($older, $newer) {

&#xA0; $Y1 = $older-&gt;format(&apos;Y&apos;);

&#xA0; $Y2 = $newer-&gt;format(&apos;Y&apos;);

&#xA0; $Y = $Y2 - $Y1;



&#xA0; $m1 = $older-&gt;format(&apos;m&apos;);

&#xA0; $m2 = $newer-&gt;format(&apos;m&apos;);

&#xA0; $m = $m2 - $m1;



&#xA0; $d1 = $older-&gt;format(&apos;d&apos;);

&#xA0; $d2 = $newer-&gt;format(&apos;d&apos;);

&#xA0; $d = $d2 - $d1;



&#xA0; $H1 = $older-&gt;format(&apos;H&apos;);

&#xA0; $H2 = $newer-&gt;format(&apos;H&apos;);

&#xA0; $H = $H2 - $H1;



&#xA0; $i1 = $older-&gt;format(&apos;i&apos;);

&#xA0; $i2 = $newer-&gt;format(&apos;i&apos;);

&#xA0; $i = $i2 - $i1;



&#xA0; $s1 = $older-&gt;format(&apos;s&apos;);

&#xA0; $s2 = $newer-&gt;format(&apos;s&apos;);

&#xA0; $s = $s2 - $s1;



&#xA0; if($s &lt; 0) {

&#xA0; &#xA0; $i = $i -1;

&#xA0; &#xA0; $s = $s + 60;

&#xA0; }

&#xA0; if($i &lt; 0) {

&#xA0; &#xA0; $H = $H - 1;

&#xA0; &#xA0; $i = $i + 60;

&#xA0; }

&#xA0; if($H &lt; 0) {

&#xA0; &#xA0; $d = $d - 1;

&#xA0; &#xA0; $H = $H + 24;

&#xA0; }

&#xA0; if($d &lt; 0) {

&#xA0; &#xA0; $m = $m - 1;

&#xA0; &#xA0; $d = $d + get_days_for_previous_month($m2, $Y2);

&#xA0; }

&#xA0; if($m &lt; 0) {

&#xA0; &#xA0; $Y = $Y - 1;

&#xA0; &#xA0; $m = $m + 12;

&#xA0; }

&#xA0; $timespan_string = create_timespan_string($Y, $m, $d, $H, $i, $s);

&#xA0; return $timespan_string;

}



function get_days_for_previous_month($current_month, $current_year) {

&#xA0; $previous_month = $current_month - 1;

&#xA0; if($current_month == 1) {

&#xA0; &#xA0; $current_year = $current_year - 1; //going from January to previous December

&#xA0; &#xA0; $previous_month = 12;

&#xA0; }

&#xA0; if($previous_month == 11 || $previous_month == 9 || $previous_month == 6 || $previous_month == 4) {

&#xA0; &#xA0; return 30;

&#xA0; }

&#xA0; else if($previous_month == 2) {

&#xA0; &#xA0; if(($current_year % 4) == 0) { //remainder 0 for leap years

&#xA0; &#xA0; &#xA0; return 29;

&#xA0; &#xA0; }

&#xA0; &#xA0; else {

&#xA0; &#xA0; &#xA0; return 28;

&#xA0; &#xA0; }

&#xA0; }

&#xA0; else {

&#xA0; &#xA0; return 31;

&#xA0; }

}



function create_timespan_string($Y, $m, $d, $H, $i, $s)

{

&#xA0; $timespan_string = &apos;&apos;;

&#xA0; $found_first_diff = false;

&#xA0; if($Y &gt;= 1) {

&#xA0; &#xA0; $found_first_diff = true;

&#xA0; &#xA0; $timespan_string .= pluralize($Y, &apos;year&apos;).&apos; &apos;;

&#xA0; }

&#xA0; if($m &gt;= 1 || $found_first_diff) {

&#xA0; &#xA0; $found_first_diff = true;

&#xA0; &#xA0; $timespan_string .= pluralize($m, &apos;month&apos;).&apos; &apos;;

&#xA0; }

&#xA0; if($d &gt;= 1 || $found_first_diff) {

&#xA0; &#xA0; $found_first_diff = true;

&#xA0; &#xA0; $timespan_string .= pluralize($d, &apos;day&apos;).&apos; &apos;;

&#xA0; }

&#xA0; if($H &gt;= 1 || $found_first_diff) {

&#xA0; &#xA0; $found_first_diff = true;

&#xA0; &#xA0; $timespan_string .= pluralize($H, &apos;hour&apos;).&apos; &apos;;

&#xA0; }

&#xA0; if($i &gt;= 1 || $found_first_diff) {

&#xA0; &#xA0; $found_first_diff = true;

&#xA0; &#xA0; $timespan_string .= pluralize($i, &apos;minute&apos;).&apos; &apos;;

&#xA0; }

&#xA0; if($found_first_diff) {

&#xA0; &#xA0; $timespan_string .= &apos;and &apos;;

&#xA0; }

&#xA0; $timespan_string .= pluralize($s, &apos;second&apos;);

&#xA0; return $timespan_string;

}



function pluralize( $count, $text )

{

&#xA0; return $count . ( ( $count == 1 ) ? ( &quot; $text&quot; ) : ( &quot; ${text}s&quot; ) );

}

?>
```



  

#



I needed to get the exact number of days between 2 dates and was relying on the this diff function, but found that I was getting a peculiar result with:



```
<?php
&#xA0; &#xA0; $today = new DateTime(date(&apos;2011-11-09&apos;));
&#xA0; &#xA0; $appt&#xA0; = new DateTime(date(&apos;2011-12-09&apos;));
&#xA0; &#xA0; $days_until_appt = $appt-&gt;diff($today)-&gt;d;
?>
```


This was returning 0 because it was exactly one month.

I had to end up using :



```
<?php
&#xA0; &#xA0; $days_until_appt = $appt-&gt;diff($today)-&gt;days;
?>
```


to get 30.

  

#

[Official documentation page](https://www.php.net/manual/en/datetime.diff.php)

**[To root](/README.md)**