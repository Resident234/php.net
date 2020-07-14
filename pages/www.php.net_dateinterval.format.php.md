# DateInterval::format



How to easy recalculate carry over points:<br><br>

```
<?php<br>class DateIntervalEnhanced extends DateInterval {<br><br>    public function recalculate()<br>    {<br>        $from = new DateTime;<br>        $to = clone $from;<br>        $to = $to-&gt;add($this);<br>        $diff = $from-&gt;diff($to);<br>        foreach ($diff as $k =&gt; $v) $this-&gt;$k = $v;<br>        return $this;<br>    }<br><br>}<br><br>$di = new DateIntervalEnhanced(&apos;PT3600S&apos;);<br>echo "Instead of " . $di-&gt;format(&apos;%h:%i:%s&apos;) . " it outputs " . $di-&gt;recalculate()-&gt;format(&apos;%h:%i:%s&apos;);<br># output will be: "Instead of 0:0:3600 it outputs 1:0:0"  

#

With php 5.3, DateTime is sweet !<br>Here is one quick example :<br><br>

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
    if(!($start instanceof DateTime)) {
        $start = new DateTime($start);
    }
    
    if($end === null) {
        $end = new DateTime();
    }
    
    if(!($end instanceof DateTime)) {
        $end = new DateTime($start);
    }
    
    $interval = $end-&gt;diff($start);
    $doPlural = function($nb,$str){return $nb&gt;1?$str.&apos;s&apos;:$str;}; // adds plurals
    
    $format = array();
    if($interval-&gt;y !== 0) {
        $format[] = "%y ".$doPlural($interval-&gt;y, "year");
    }
    if($interval-&gt;m !== 0) {
        $format[] = "%m ".$doPlural($interval-&gt;m, "month");
    }
    if($interval-&gt;d !== 0) {
        $format[] = "%d ".$doPlural($interval-&gt;d, "day");
    }
    if($interval-&gt;h !== 0) {
        $format[] = "%h ".$doPlural($interval-&gt;h, "hour");
    }
    if($interval-&gt;i !== 0) {
        $format[] = "%i ".$doPlural($interval-&gt;i, "minute");
    }
    if($interval-&gt;s !== 0) {
        if(!count($format)) {
            return "less than a minute ago";
        } else {
            $format[] = "%s ".$doPlural($interval-&gt;s, "second");
        }
    }
    
    // We use the two biggest parts
    if(count($format) &gt; 1) {
        $format = array_shift($format)." and ".array_shift($format);
    } else {
        $format = array_pop($format);
    }
    
    // Prepend &apos;since &apos; or whatever you like
    return $interval-&gt;format($format);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/dateinterval.format.php)

**[To root](/README.md)**