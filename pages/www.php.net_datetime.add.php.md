# DateTime::add



Note that the add() and sub() methods will modify the value of the object you&apos;re calling the method on! This is very untypical for a method that returns a value of its own type. You could misunderstand it that the method would return a new instance with the modified value, but in fact it modifies itself! This is undocumented here. (Only a side note on procedural style mentions it, but it obviously does not apply to object oriented style.)  

#

Another simple solution to adding a month but not autocorrecting days to the next month is this.<br>(Also works for substracting months)<br><br>$dt = new DateTime("2016-01-31");<br><br>$oldDay = $dt-&gt;format("d");<br>$dt-&gt;add(new DateInterval("P1M")); // 2016-03-02<br>$newDay = $dt-&gt;format("d");<br><br>if($oldDay != $newDay) {<br>    // Check if the day is changed, if so we skipped to the next month.<br>    // Substract days to go back to the last day of previous month.<br>    $dt-&gt;sub(new DateInterval("P" . $newDay . "D"));<br>}<br><br>echo $dt-&gt;format("Y-m-d"); // 2016-02-29<br><br>Hope this helps someone.  

#

If you&apos;re using PHP &gt;= 5.5, instead of using "glavic at gmail dot com"&apos;s DateTimeEnhanced class, use the built in DateTimeImmutable type. When you call DateTimeImmutable::add() it will return a new object, rather than modifying the original  

#

Here is a solution to adding months when you want 2014-10-31 to become 2014-11-30 instead of 2014-12-01.<br><br>

```
<?php

/**
 * Class MyDateTime
 *
 * Extends DateTime to include a sensible addMonth method.
 *
 * This class provides a method that will increment the month, and
 * if the day is greater than the last day in the new month, it
 * changes the day to the last day of that month. For example,
 * If you add one month to 2014-10-31 using DateTime::add, the
 * result is 2014-12-01. Using MyDateTime::addMonth the result is
 * 2014-11-30.
 */
class MyDateTime extends DateTime
{

    public function addMonth($num = 1)
    {
        $date = $this-&gt;format(&apos;Y-n-j&apos;);
        list($y, $m, $d) = explode(&apos;-&apos;, $date);

        $m += $num;
        while ($m &gt; 12)
        {
            $m -= 12;
            $y++;
        }

        $last_day = date(&apos;t&apos;, strtotime("$y-$m-1"));
        if ($d &gt; $last_day)
        {
            $d = $last_day;
        }

        $this-&gt;setDate($y, $m, $d);
    }

}

?>
```
  

#

If you need add() and sub() that don&apos;t modify object values, you can create new methods like this:<br><br>

```
<?php<br><br>class DateTimeEnhanced extends DateTime {<br><br>    public function returnAdd(DateInterval $interval)<br>    {<br>        $dt = clone $this;<br>        $dt-&gt;add($interval);<br>        return $dt;<br>    }<br>    <br>    public function returnSub(DateInterval $interval)<br>    {<br>        $dt = clone $this;<br>        $dt-&gt;sub($interval);<br>        return $dt;<br>    }<br><br>}<br><br>$interval = DateInterval::createfromdatestring(&apos;+1 day&apos;);<br><br>$dt = new DateTimeEnhanced; # initialize new object<br>echo $dt-&gt;format(DateTime::W3C) . "\n"; # 2013-09-12T15:01:44+02:00<br><br>$dt-&gt;add($interval); # this modifies the object values<br>echo $dt-&gt;format(DateTime::W3C) . "\n"; # 2013-09-13T15:01:44+02:00<br><br>$dtNew = $dt-&gt;returnAdd($interval); # this returns the new modified object and doesn&apos;t change original object<br>echo $dt-&gt;format(DateTime::W3C) . "\n"; # 2013-09-13T15:01:44+02:00<br>echo $dtNew-&gt;format(DateTime::W3C) . "\n"; # 2013-09-14T15:01:44+02:00  

#

[Official documentation page](https://www.php.net/manual/en/datetime.add.php)

**[To root](/README.md)**