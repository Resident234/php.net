# time





The documentation should have this info. The function time() returns always timestamp that is timezone independent (=UTC).





```
<?php

date_default_timezone_set(&quot;UTC&quot;); 

echo &quot;UTC:&quot;.time();

echo &quot;&lt;br&gt;&quot;;



date_default_timezone_set(&quot;Europe/Helsinki&quot;); 

echo &quot;Europe/Helsinki:&quot;.time();

echo &quot;&lt;br&gt;&quot;;

?>
```




Local time as string can be get by strftime() and local timestamp (if ever needed) by mktime().

  

#



Two quick approaches to getting the time elapsed in human readable form.



```
<?php

function time_elapsed_A($secs){
&#xA0; &#xA0; $bit = array(
&#xA0; &#xA0; &#xA0; &#xA0; &apos;y&apos; =&gt; $secs / 31556926 % 12,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;w&apos; =&gt; $secs / 604800 % 52,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;d&apos; =&gt; $secs / 86400 % 7,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;h&apos; =&gt; $secs / 3600 % 24,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;m&apos; =&gt; $secs / 60 % 60,
&#xA0; &#xA0; &#xA0; &#xA0; &apos;s&apos; =&gt; $secs % 60
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; foreach($bit as $k =&gt; $v)
&#xA0; &#xA0; &#xA0; &#xA0; if($v &gt; 0)$ret[] = $v . $k;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; return join(&apos; &apos;, $ret);
&#xA0; &#xA0; }
&#xA0; &#xA0; 

function time_elapsed_B($secs){
&#xA0; &#xA0; $bit = array(
&#xA0; &#xA0; &#xA0; &#xA0; &apos; year&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; $secs / 31556926 % 12,
&#xA0; &#xA0; &#xA0; &#xA0; &apos; week&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; $secs / 604800 % 52,
&#xA0; &#xA0; &#xA0; &#xA0; &apos; day&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; $secs / 86400 % 7,
&#xA0; &#xA0; &#xA0; &#xA0; &apos; hour&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt; $secs / 3600 % 24,
&#xA0; &#xA0; &#xA0; &#xA0; &apos; minute&apos;&#xA0; &#xA0; =&gt; $secs / 60 % 60,
&#xA0; &#xA0; &#xA0; &#xA0; &apos; second&apos;&#xA0; &#xA0; =&gt; $secs % 60
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; foreach($bit as $k =&gt; $v){
&#xA0; &#xA0; &#xA0; &#xA0; if($v &gt; 1)$ret[] = $v . $k . &apos;s&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; if($v == 1)$ret[] = $v . $k;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; array_splice($ret, count($ret)-1, 0, &apos;and&apos;);
&#xA0; &#xA0; $ret[] = &apos;ago.&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; return join(&apos; &apos;, $ret);
&#xA0; &#xA0; }
&#xA0; &#xA0; 

&#xA0; &#xA0; 
&#xA0; &#xA0; 
$nowtime = time();
$oldtime = 1335939007;

echo &quot;time_elapsed_A: &quot;.time_elapsed_A($nowtime-$oldtime).&quot;\n&quot;;
echo &quot;time_elapsed_B: &quot;.time_elapsed_B($nowtime-$oldtime).&quot;\n&quot;;

/** Output:
time_elapsed_A: 6d 15h 48m 19s
time_elapsed_B: 6 days 15 hours 48 minutes and 19 seconds ago.
**/
?>
```



  

#



The number of 86,400 seconds in a day comes from the assumption of 60 seconds per minute, 60 minutes per hour and 24 hours per day: 60*60*24.&#xA0; But not every day has 24 hours.&#xA0; This edge case occurs on the days that clocks change as we enter and leave daylight savings (summer) time.&#xA0; Date and time arithmetic is logically consistent and correct when you use PHP built-in functions, but it may not always work as expected if you try to write your own date and time arithmetic.

  

#



A time difference function that outputs the time passed in facebook&apos;s style: 1 day ago, or 4 months ago. I took andrew dot macrobert at gmail dot com function and tweaked it a bit. On a strict enviroment it was throwing errors, plus I needed it to calculate the difference in time between a past date and a future date. 



```
<?php
function nicetime($date)
{
&#xA0; &#xA0; if(empty($date)) {
&#xA0; &#xA0; &#xA0; &#xA0; return &quot;No date provided&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; $periods&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = array(&quot;second&quot;, &quot;minute&quot;, &quot;hour&quot;, &quot;day&quot;, &quot;week&quot;, &quot;month&quot;, &quot;year&quot;, &quot;decade&quot;);
&#xA0; &#xA0; $lengths&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = array(&quot;60&quot;,&quot;60&quot;,&quot;24&quot;,&quot;7&quot;,&quot;4.35&quot;,&quot;12&quot;,&quot;10&quot;);
&#xA0; &#xA0; 
&#xA0; &#xA0; $now&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = time();
&#xA0; &#xA0; $unix_date&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = strtotime($date);
&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0;&#xA0; // check validity of date
&#xA0; &#xA0; if(empty($unix_date)) {&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; return &quot;Bad date&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; // is it future date or past date
&#xA0; &#xA0; if($now &gt; $unix_date) {&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $difference&#xA0; &#xA0;&#xA0; = $now - $unix_date;
&#xA0; &#xA0; &#xA0; &#xA0; $tense&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = &quot;ago&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; $difference&#xA0; &#xA0;&#xA0; = $unix_date - $now;
&#xA0; &#xA0; &#xA0; &#xA0; $tense&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = &quot;from now&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; for($j = 0; $difference &gt;= $lengths[$j] &amp;&amp; $j &lt; count($lengths)-1; $j++) {
&#xA0; &#xA0; &#xA0; &#xA0; $difference /= $lengths[$j];
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; $difference = round($difference);
&#xA0; &#xA0; 
&#xA0; &#xA0; if($difference != 1) {
&#xA0; &#xA0; &#xA0; &#xA0; $periods[$j].= &quot;s&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; return &quot;$difference $periods[$j] {$tense}&quot;;
}

$date = &quot;2009-03-04 17:45&quot;;
$result = nicetime($date); // 2 days ago

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.time.php)

**[To root](/README.md)**