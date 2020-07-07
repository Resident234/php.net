# DateInterval::format





How to easy recalculate carry over points:



```
<?php
class DateIntervalEnhanced extends DateInterval {

&#xA0; &#xA0; public function recalculate()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $from = new DateTime;
&#xA0; &#xA0; &#xA0; &#xA0; $to = clone $from;
&#xA0; &#xA0; &#xA0; &#xA0; $to = $to-&gt;add($this);
&#xA0; &#xA0; &#xA0; &#xA0; $diff = $from-&gt;diff($to);
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($diff as $k =&gt; $v) $this-&gt;$k = $v;
&#xA0; &#xA0; &#xA0; &#xA0; return $this;
&#xA0; &#xA0; }

}

$di = new DateIntervalEnhanced(&apos;PT3600S&apos;);
echo &quot;Instead of &quot; . $di-&gt;format(&apos;%h:%i:%s&apos;) . &quot; it outputs &quot; . $di-&gt;recalculate()-&gt;format(&apos;%h:%i:%s&apos;);
# output will be: &quot;Instead of 0:0:3600 it outputs 1:0:0&quot;


  

#



With php 5.3, DateTime is sweet !

Here is one quick example :





```
<?php

/**

 * A sweet interval formatting, will use the two biggest interval parts.

 * On small intervals, you get minutes and seconds.

 * On big intervals, you get months and days.

 * Only the two biggest parts are used. 

 * 

 * @param DateTime $start

 * @param DateTime|null $end

 * @return string

 */

public function formatDateDiff($start, $end=null) {

&#xA0; &#xA0; if(!($start instanceof DateTime)) {

&#xA0; &#xA0; &#xA0; &#xA0; $start = new DateTime($start);

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; if($end === null) {

&#xA0; &#xA0; &#xA0; &#xA0; $end = new DateTime();

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; if(!($end instanceof DateTime)) {

&#xA0; &#xA0; &#xA0; &#xA0; $end = new DateTime($start);

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; $interval = $end-&gt;diff($start);

&#xA0; &#xA0; $doPlural = function($nb,$str){return $nb&gt;1?$str.&apos;s&apos;:$str;}; // adds plurals

&#xA0; &#xA0; 

&#xA0; &#xA0; $format = array();

&#xA0; &#xA0; if($interval-&gt;y !== 0) {

&#xA0; &#xA0; &#xA0; &#xA0; $format[] = &quot;%y &quot;.$doPlural($interval-&gt;y, &quot;year&quot;);

&#xA0; &#xA0; }

&#xA0; &#xA0; if($interval-&gt;m !== 0) {

&#xA0; &#xA0; &#xA0; &#xA0; $format[] = &quot;%m &quot;.$doPlural($interval-&gt;m, &quot;month&quot;);

&#xA0; &#xA0; }

&#xA0; &#xA0; if($interval-&gt;d !== 0) {

&#xA0; &#xA0; &#xA0; &#xA0; $format[] = &quot;%d &quot;.$doPlural($interval-&gt;d, &quot;day&quot;);

&#xA0; &#xA0; }

&#xA0; &#xA0; if($interval-&gt;h !== 0) {

&#xA0; &#xA0; &#xA0; &#xA0; $format[] = &quot;%h &quot;.$doPlural($interval-&gt;h, &quot;hour&quot;);

&#xA0; &#xA0; }

&#xA0; &#xA0; if($interval-&gt;i !== 0) {

&#xA0; &#xA0; &#xA0; &#xA0; $format[] = &quot;%i &quot;.$doPlural($interval-&gt;i, &quot;minute&quot;);

&#xA0; &#xA0; }

&#xA0; &#xA0; if($interval-&gt;s !== 0) {

&#xA0; &#xA0; &#xA0; &#xA0; if(!count($format)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &quot;less than a minute ago&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $format[] = &quot;%s &quot;.$doPlural($interval-&gt;s, &quot;second&quot;);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; // We use the two biggest parts

&#xA0; &#xA0; if(count($format) &gt; 1) {

&#xA0; &#xA0; &#xA0; &#xA0; $format = array_shift($format).&quot; and &quot;.array_shift($format);

&#xA0; &#xA0; } else {

&#xA0; &#xA0; &#xA0; &#xA0; $format = array_pop($format);

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; // Prepend &apos;since &apos; or whatever you like

&#xA0; &#xA0; return $interval-&gt;format($format);

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/dateinterval.format.php)

**[To root](/README.md)**