# DateTime::modify



a slightly more compact way of getting the month shift<br><br>

```
<?php<br><br>      /**<br>     * correctly calculates end of months when we shift to a shorter or longer month<br>     * workaround for http://php.net/manual/en/datetime.add.php#example-2489 <br>     * <br>     * Makes the assumption that shifting from the 28th Feb +1 month is 31st March<br>     * Makes the assumption that shifting from the 28th Feb -1 month is 31st Jan<br>     * Makes the assumption that shifting from the 29,30,31 Jan +1 month is 28th (or 29th) Feb<br>     *<br>     * <br>     * @param DateTime $aDate<br>     * @param int $months positive or negative<br>     * <br>     * @return DateTime new instance - original parameter is unchanged<br>     */<br><br>    function MonthShifter (DateTime $aDate,$months){<br>        $dateA = clone($aDate);<br>        $dateB = clone($aDate);<br>        $plusMonths = clone($dateA-&gt;modify($months . &apos; Month&apos;));<br>        //check whether reversing the month addition gives us the original day back<br>        if($dateB != $dateA-&gt;modify($months*-1 . &apos; Month&apos;)){ <br>            $result = $plusMonths-&gt;modify(&apos;last day of last month&apos;);<br>        } elseif($aDate == $dateB-&gt;modify(&apos;last day of this month&apos;)){<br>            $result =  $plusMonths-&gt;modify(&apos;last day of this month&apos;);<br>        } else {<br>            $result = $plusMonths;<br>        }<br>        return $result;<br>    }<br><br>//TEST<br><br>$x = new DateTime(&apos;2017-01-30&apos;);<br>echo( $x-&gt;format(&apos;Y-m-d&apos;)." past end of feb, but not dec&lt;br&gt;");<br>echo(&apos;b &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br>echo(&apos;c &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br><br>$x = new DateTime(&apos;2017-01-15&apos;);<br>echo("&lt;br&gt;" . $x-&gt;format(&apos;Y-m-d&apos;)." middle of the month &lt;br&gt;");<br>echo(&apos;d &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br>echo(&apos;e &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br><br>$x = new DateTime(&apos;2017-02-28&apos;);<br>echo("&lt;br&gt;" . $x-&gt;format(&apos;Y-m-d&apos;)." end of Feb&lt;br&gt;");<br>echo(&apos;f &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br>echo(&apos;g &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br><br>$x = new DateTime(&apos;2017-01-31&apos;);<br>echo("&lt;br&gt;" .  $x-&gt;format(&apos;Y-m-d&apos;)." end of Jan&lt;br&gt;");<br>echo(&apos;h &apos; . MonthShifter($x,1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br>echo(&apos;i &apos; . MonthShifter($x,-1)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br><br>$x = new DateTime(&apos;2017-01-31&apos;);<br>echo("&lt;br&gt;" .  $x-&gt;format(&apos;Y-m-d&apos;)." end of Jan +/- 1 years diff, leap year respected&lt;br&gt;");<br>echo(&apos;j &apos; . MonthShifter($x,13)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br>echo(&apos;k &apos; . MonthShifter($x,-11)-&gt;format((&apos;Y-m-d&apos;))."&lt;br&gt;");<br><br>//returns<br><br>2017-01-30 past end of feb, but not dec<br>b 2017-02-28<br>c 2016-12-30<br><br>2017-01-15 middle of the month <br>d 2017-02-15<br>e 2016-12-15<br><br>2017-02-28end of Feb<br>f 2017-03-31<br>g 2017-01-31<br><br>2017-01-31end of Jan<br>h 2017-02-28<br>i 2016-12-31<br><br>2017-01-31end of Jan +/- 1 years diff, leap year respected<br>j 2018-02-28<br>k 2016-02-29  

#

These functions makes sure that adding months or years always ends up in the month you would expect.  Works for positive and negative values<br><br>

```
<?php
      
       
    $date=new DateTime();
    $date-&gt;setDate(2008,2,29);
    
    function addMonths($date,$months){
         
        $init=clone $date;
        $modifier=$months.&apos; months&apos;;
        $back_modifier =-$months.&apos; months&apos;;
        
        $date-&gt;modify($modifier);
        $back_to_init= clone $date;
        $back_to_init-&gt;modify($back_modifier);
        
        while($init-&gt;format(&apos;m&apos;)!=$back_to_init-&gt;format(&apos;m&apos;)){
        $date-&gt;modify(&apos;-1 day&apos;)    ;
        $back_to_init= clone $date;
        $back_to_init-&gt;modify($back_modifier);    
        }
        
        /*
        if($months&lt;0&amp;&amp;$date-&gt;format(&apos;m&apos;)&gt;$init-&gt;format(&apos;m&apos;))
        while($date-&gt;format(&apos;m&apos;)-12-$init-&gt;format(&apos;m&apos;)!=$months%12)
        $date-&gt;modify(&apos;-1 day&apos;);
        else
        if($months&gt;0&amp;&amp;$date-&gt;format(&apos;m&apos;)&lt;$init-&gt;format(&apos;m&apos;))
        while($date-&gt;format(&apos;m&apos;)+12-$init-&gt;format(&apos;m&apos;)!=$months%12)
        $date-&gt;modify(&apos;-1 day&apos;);
        else
        while($date-&gt;format(&apos;m&apos;)-$init-&gt;format(&apos;m&apos;)!=$months%12)
        $date-&gt;modify(&apos;-1 day&apos;);
        */
        
    }
     
    function addYears($date,$years){
        
        $init=clone $date;
        $modifier=$years.&apos; years&apos;;
        $date-&gt;modify($modifier);
        
        while($date-&gt;format(&apos;m&apos;)!=$init-&gt;format(&apos;m&apos;))
        $date-&gt;modify(&apos;-1 day&apos;);
        
        
    } 
    
    
    
    addMonths($date,-1);
     addYears($date,3);
    
    
    echo $date-&gt;format(&apos;F j,Y&apos;);
     
 
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.modify.php)

**[To root](/README.md)**