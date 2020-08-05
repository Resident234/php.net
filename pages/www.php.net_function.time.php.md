# time



The documentation should have this info. The function time() returns always timestamp that is timezone independent (=UTC).<br><br>

```
<?php
date_default_timezone_set("UTC"); 
echo "UTC:".time();
echo "<br>";

date_default_timezone_set("Europe/Helsinki"); 
echo "Europe/Helsinki:".time();
echo "<br>";
?>
```
<br><br>Local time as string can be get by strftime() and local timestamp (if ever needed) by mktime().  

---

Two quick approaches to getting the time elapsed in human readable form.<br><br>

```
<?php

function time_elapsed_A($secs){
    $bit = array(
        'y' => $secs / 31556926 % 12,
        'w' => $secs / 604800 % 52,
        'd' => $secs / 86400 % 7,
        'h' => $secs / 3600 % 24,
        'm' => $secs / 60 % 60,
        's' => $secs % 60
        );
        
    foreach($bit as $k => $v)
        if($v > 0)$ret[] = $v . $k;
        
    return join(' ', $ret);
    }
    

function time_elapsed_B($secs){
    $bit = array(
        ' year'        => $secs / 31556926 % 12,
        ' week'        => $secs / 604800 % 52,
        ' day'        => $secs / 86400 % 7,
        ' hour'        => $secs / 3600 % 24,
        ' minute'    => $secs / 60 % 60,
        ' second'    => $secs % 60
        );
        
    foreach($bit as $k => $v){
        if($v > 1)$ret[] = $v . $k . 's';
        if($v == 1)$ret[] = $v . $k;
        }
    array_splice($ret, count($ret)-1, 0, 'and');
    $ret[] = 'ago.';
    
    return join(' ', $ret);
    }
    

    
    
$nowtime = time();
$oldtime = 1335939007;

echo "time_elapsed_A: ".time_elapsed_A($nowtime-$oldtime)."\n";
echo "time_elapsed_B: ".time_elapsed_B($nowtime-$oldtime)."\n";

/** Output:
time_elapsed_A: 6d 15h 48m 19s
time_elapsed_B: 6 days 15 hours 48 minutes and 19 seconds ago.
**/
?>
```
  

---

The number of 86,400 seconds in a day comes from the assumption of 60 seconds per minute, 60 minutes per hour and 24 hours per day: 60*60*24.  But not every day has 24 hours.  This edge case occurs on the days that clocks change as we enter and leave daylight savings (summer) time.  Date and time arithmetic is logically consistent and correct when you use PHP built-in functions, but it may not always work as expected if you try to write your own date and time arithmetic.  

---

A time difference function that outputs the time passed in facebook&apos;s style: 1 day ago, or 4 months ago. I took andrew dot macrobert at gmail dot com function and tweaked it a bit. On a strict enviroment it was throwing errors, plus I needed it to calculate the difference in time between a past date and a future date. <br><br>

```
<?php
function nicetime($date)
{
    if(empty($date)) {
        return "No date provided";
    }
    
    $periods         = array("second", "minute", "hour", "day", "week", "month", "year", "decade");
    $lengths         = array("60","60","24","7","4.35","12","10");
    
    $now             = time();
    $unix_date         = strtotime($date);
    
       // check validity of date
    if(empty($unix_date)) {    
        return "Bad date";
    }

    // is it future date or past date
    if($now > $unix_date) {    
        $difference     = $now - $unix_date;
        $tense         = "ago";
        
    } else {
        $difference     = $unix_date - $now;
        $tense         = "from now";
    }
    
    for($j = 0; $difference >= $lengths[$j] &amp;&amp; $j < count($lengths)-1; $j++) {
        $difference /= $lengths[$j];
    }
    
    $difference = round($difference);
    
    if($difference != 1) {
        $periods[$j].= "s";
    }
    
    return "$difference $periods[$j] {$tense}";
}

$date = "2009-03-04 17:45";
$result = nicetime($date); // 2 days ago

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.time.php)

**[To root](/README.md)**