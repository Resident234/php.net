# DateInterval::format



How to easy recalculate carry over points:<br><br>

```
<?php
class DateIntervalEnhanced extends DateInterval {

    public function recalculate()
    {
        $from = new DateTime;
        $to = clone $from;
        $to = $to->add($this);
        $diff = $from->diff($to);
        foreach ($diff as $k => $v) $this->$k = $v;
        return $this;
    }

}

$di = new DateIntervalEnhanced('PT3600S');
echo "Instead of " . $di->format('%h:%i:%s') . " it outputs " . $di->recalculate()->format('%h:%i:%s');
# output will be: "Instead of 0:0:3600 it outputs 1:0:0"?>
```
  

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
    
    $interval = $end->diff($start);
    $doPlural = function($nb,$str){return $nb&gt;1?$str.'s':$str;}; // adds plurals
    
    $format = array();
    if($interval->y !== 0) {
        $format[] = "%y ".$doPlural($interval->y, "year");
    }
    if($interval->m !== 0) {
        $format[] = "%m ".$doPlural($interval->m, "month");
    }
    if($interval->d !== 0) {
        $format[] = "%d ".$doPlural($interval->d, "day");
    }
    if($interval->h !== 0) {
        $format[] = "%h ".$doPlural($interval->h, "hour");
    }
    if($interval->i !== 0) {
        $format[] = "%i ".$doPlural($interval->i, "minute");
    }
    if($interval->s !== 0) {
        if(!count($format)) {
            return "less than a minute ago";
        } else {
            $format[] = "%s ".$doPlural($interval->s, "second");
        }
    }
    
    // We use the two biggest parts
    if(count($format) &gt; 1) {
        $format = array_shift($format)." and ".array_shift($format);
    } else {
        $format = array_pop($format);
    }
    
    // Prepend 'since ' or whatever you like
    return $interval->format($format);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/dateinterval.format.php)

**[To root](/README.md)**