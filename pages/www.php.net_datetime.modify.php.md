# DateTime::modify





a slightly more compact way of getting the month shift



```
<?php

&#xA0; &#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * correctly calculates end of months when we shift to a shorter or longer month
&#xA0; &#xA0;&#xA0; * workaround for http://php.net/manual/en/datetime.add.php#example-2489 
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * Makes the assumption that shifting from the 28th Feb +1 month is 31st March
&#xA0; &#xA0;&#xA0; * Makes the assumption that shifting from the 28th Feb -1 month is 31st Jan
&#xA0; &#xA0;&#xA0; * Makes the assumption that shifting from the 29,30,31 Jan +1 month is 28th (or 29th) Feb
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * @param DateTime $aDate
&#xA0; &#xA0;&#xA0; * @param int $months positive or negative
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * @return DateTime new instance - original parameter is unchanged
&#xA0; &#xA0;&#xA0; */

&#xA0; &#xA0; function MonthShifter (DateTime $aDate,$months){
&#xA0; &#xA0; &#xA0; &#xA0; $dateA = clone($aDate);
&#xA0; &#xA0; &#xA0; &#xA0; $dateB = clone($aDate);
&#xA0; &#xA0; &#xA0; &#xA0; $plusMonths = clone($dateA-&gt;modify($months . &apos; Month&apos;));
&#xA0; &#xA0; &#xA0; &#xA0; //check whether reversing the month addition gives us the original day back
&#xA0; &#xA0; &#xA0; &#xA0; if($dateB != $dateA-&gt;modify($months*-1 . &apos; Month&apos;)){ 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = $plusMonths-&gt;modify(&apos;last day of last month&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; } elseif($aDate == $dateB-&gt;modify(&apos;last day of this month&apos;)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result =&#xA0; $plusMonths-&gt;modify(&apos;last day of this month&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = $plusMonths;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $result;
&#xA0; &#xA0; }

//TEST

$x = new DateTime(&apos;2017-01-30&apos;);
echo( $x-&gt;format(&apos;Y-m-d&apos;).&quot; past end of feb, but not dec&lt;br&gt;&quot;);
echo(&apos;b &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);
echo(&apos;c &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);

$x = new DateTime(&apos;2017-01-15&apos;);
echo(&quot;&lt;br&gt;&quot; . $x-&gt;format(&apos;Y-m-d&apos;).&quot; middle of the month &lt;br&gt;&quot;);
echo(&apos;d &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);
echo(&apos;e &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);

$x = new DateTime(&apos;2017-02-28&apos;);
echo(&quot;&lt;br&gt;&quot; . $x-&gt;format(&apos;Y-m-d&apos;).&quot; end of Feb&lt;br&gt;&quot;);
echo(&apos;f &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);
echo(&apos;g &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);

$x = new DateTime(&apos;2017-01-31&apos;);
echo(&quot;&lt;br&gt;&quot; .&#xA0; $x-&gt;format(&apos;Y-m-d&apos;).&quot; end of Jan&lt;br&gt;&quot;);
echo(&apos;h &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);
echo(&apos;i &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);

$x = new DateTime(&apos;2017-01-31&apos;);
echo(&quot;&lt;br&gt;&quot; .&#xA0; $x-&gt;format(&apos;Y-m-d&apos;).&quot; end of Jan +/- 1 years diff, leap year respected&lt;br&gt;&quot;);
echo(&apos;j &apos; . MonthShifter($x,13)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);
echo(&apos;k &apos; . MonthShifter($x,-11)-&gt;format((&apos;Y-m-d&apos;)).&quot;&lt;br&gt;&quot;);

//returns

2017-01-30 past end of feb, but not dec
b 2017-02-28
c 2016-12-30

2017-01-15 middle of the month 
d 2017-02-15
e 2016-12-15

2017-02-28end of Feb
f 2017-03-31
g 2017-01-31

2017-01-31end of Jan
h 2017-02-28
i 2016-12-31

2017-01-31end of Jan +/- 1 years diff, leap year respected
j 2018-02-28
k 2016-02-29


  

#



These functions makes sure that adding months or years always ends up in the month you would expect.&#xA0; Works for positive and negative values



```
<?php
&#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; $date=new DateTime();
&#xA0; &#xA0; $date-&gt;setDate(2008,2,29);
&#xA0; &#xA0; 
&#xA0; &#xA0; function addMonths($date,$months){
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $init=clone $date;
&#xA0; &#xA0; &#xA0; &#xA0; $modifier=$months.&apos; months&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $back_modifier =-$months.&apos; months&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify($modifier);
&#xA0; &#xA0; &#xA0; &#xA0; $back_to_init= clone $date;
&#xA0; &#xA0; &#xA0; &#xA0; $back_to_init-&gt;modify($back_modifier);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; while($init-&gt;format(&apos;m&apos;)!=$back_to_init-&gt;format(&apos;m&apos;)){
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify(&apos;-1 day&apos;)&#xA0; &#xA0; ;
&#xA0; &#xA0; &#xA0; &#xA0; $back_to_init= clone $date;
&#xA0; &#xA0; &#xA0; &#xA0; $back_to_init-&gt;modify($back_modifier);&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; /*
&#xA0; &#xA0; &#xA0; &#xA0; if($months&lt;0&amp;&amp;$date-&gt;format(&apos;m&apos;)&gt;$init-&gt;format(&apos;m&apos;))
&#xA0; &#xA0; &#xA0; &#xA0; while($date-&gt;format(&apos;m&apos;)-12-$init-&gt;format(&apos;m&apos;)!=$months%12)
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify(&apos;-1 day&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; if($months&gt;0&amp;&amp;$date-&gt;format(&apos;m&apos;)&lt;$init-&gt;format(&apos;m&apos;))
&#xA0; &#xA0; &#xA0; &#xA0; while($date-&gt;format(&apos;m&apos;)+12-$init-&gt;format(&apos;m&apos;)!=$months%12)
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify(&apos;-1 day&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; while($date-&gt;format(&apos;m&apos;)-$init-&gt;format(&apos;m&apos;)!=$months%12)
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify(&apos;-1 day&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; */
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }
&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; function addYears($date,$years){
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $init=clone $date;
&#xA0; &#xA0; &#xA0; &#xA0; $modifier=$years.&apos; years&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify($modifier);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; while($date-&gt;format(&apos;m&apos;)!=$init-&gt;format(&apos;m&apos;))
&#xA0; &#xA0; &#xA0; &#xA0; $date-&gt;modify(&apos;-1 day&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; } 
&#xA0; &#xA0; 
&#xA0; &#xA0; 
&#xA0; &#xA0; 
&#xA0; &#xA0; addMonths($date,-1);
&#xA0; &#xA0;&#xA0; addYears($date,3);
&#xA0; &#xA0; 
&#xA0; &#xA0; 
&#xA0; &#xA0; echo $date-&gt;format(&apos;F j,Y&apos;);
&#xA0; &#xA0;&#xA0; 
 
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/datetime.modify.php)

**[To root](/README.md)**