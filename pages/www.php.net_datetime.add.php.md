# DateTime::add





Note that the add() and sub() methods will modify the value of the object you&apos;re calling the method on! This is very untypical for a method that returns a value of its own type. You could misunderstand it that the method would return a new instance with the modified value, but in fact it modifies itself! This is undocumented here. (Only a side note on procedural style mentions it, but it obviously does not apply to object oriented style.)

  

#



Another simple solution to adding a month but not autocorrecting days to the next month is this.
(Also works for substracting months)

$dt = new DateTime(&quot;2016-01-31&quot;);

$oldDay = $dt-&gt;format(&quot;d&quot;);
$dt-&gt;add(new DateInterval(&quot;P1M&quot;)); // 2016-03-02
$newDay = $dt-&gt;format(&quot;d&quot;);

if($oldDay != $newDay) {
&#xA0; &#xA0; // Check if the day is changed, if so we skipped to the next month.
&#xA0; &#xA0; // Substract days to go back to the last day of previous month.
&#xA0; &#xA0; $dt-&gt;sub(new DateInterval(&quot;P&quot; . $newDay . &quot;D&quot;));
}

echo $dt-&gt;format(&quot;Y-m-d&quot;); // 2016-02-29

Hope this helps someone.

  

#



If you&apos;re using PHP &gt;= 5.5, instead of using &quot;glavic at gmail dot com&quot;&apos;s DateTimeEnhanced class, use the built in DateTimeImmutable type. When you call DateTimeImmutable::add() it will return a new object, rather than modifying the original

  

#



Here is a solution to adding months when you want 2014-10-31 to become 2014-11-30 instead of 2014-12-01.



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

&#xA0; &#xA0; public function addMonth($num = 1)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $date = $this-&gt;format(&apos;Y-n-j&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; list($y, $m, $d) = explode(&apos;-&apos;, $date);

&#xA0; &#xA0; &#xA0; &#xA0; $m += $num;
&#xA0; &#xA0; &#xA0; &#xA0; while ($m &gt; 12)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $m -= 12;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $y++;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $last_day = date(&apos;t&apos;, strtotime(&quot;$y-$m-1&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; if ($d &gt; $last_day)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $d = $last_day;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;setDate($y, $m, $d);
&#xA0; &#xA0; }

}

?>
```



  

#



If you need add() and sub() that don&apos;t modify object values, you can create new methods like this:



```
<?php

class DateTimeEnhanced extends DateTime {

&#xA0; &#xA0; public function returnAdd(DateInterval $interval)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $dt = clone $this;
&#xA0; &#xA0; &#xA0; &#xA0; $dt-&gt;add($interval);
&#xA0; &#xA0; &#xA0; &#xA0; return $dt;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function returnSub(DateInterval $interval)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $dt = clone $this;
&#xA0; &#xA0; &#xA0; &#xA0; $dt-&gt;sub($interval);
&#xA0; &#xA0; &#xA0; &#xA0; return $dt;
&#xA0; &#xA0; }

}

$interval = DateInterval::createfromdatestring(&apos;+1 day&apos;);

$dt = new DateTimeEnhanced; # initialize new object
echo $dt-&gt;format(DateTime::W3C) . &quot;\n&quot;; # 2013-09-12T15:01:44+02:00

$dt-&gt;add($interval); # this modifies the object values
echo $dt-&gt;format(DateTime::W3C) . &quot;\n&quot;; # 2013-09-13T15:01:44+02:00

$dtNew = $dt-&gt;returnAdd($interval); # this returns the new modified object and doesn&apos;t change original object
echo $dt-&gt;format(DateTime::W3C) . &quot;\n&quot;; # 2013-09-13T15:01:44+02:00
echo $dtNew-&gt;format(DateTime::W3C) . &quot;\n&quot;; # 2013-09-14T15:01:44+02:00


  

#

[Official documentation page](https://www.php.net/manual/en/datetime.add.php)

**[To root](/README.md)**