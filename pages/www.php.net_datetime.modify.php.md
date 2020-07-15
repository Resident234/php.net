# DateTime::modify



a slightly more compact way of getting the month shift<br><br>

```
<?php

      /**
     * correctly calculates end of months when we shift to a shorter or longer month
     * workaround for http://php.net/manual/en/datetime.add.php#example-2489 
     * 
     * Makes the assumption that shifting from the 28th Feb +1 month is 31st March
     * Makes the assumption that shifting from the 28th Feb -1 month is 31st Jan
     * Makes the assumption that shifting from the 29,30,31 Jan +1 month is 28th (or 29th) Feb
     *
     * 
     * @param DateTime $aDate
     * @param int $months positive or negative
     * 
     * @return DateTime new instance - original parameter is unchanged
     */

    function MonthShifter (DateTime $aDate,$months){
        $dateA = clone($aDate);
        $dateB = clone($aDate);
        $plusMonths = clone($dateA->modify($months . ' Month'));
        //check whether reversing the month addition gives us the original day back
        if($dateB != $dateA->modify($months*-1 . ' Month')){ 
            $result = $plusMonths->modify('last day of last month');
        } elseif($aDate == $dateB->modify('last day of this month')){
            $result =  $plusMonths->modify('last day of this month');
        } else {
            $result = $plusMonths;
        }
        return $result;
    }

//TEST

$x = new DateTime('2017-01-30');
echo( $x->format('Y-m-d')." past end of feb, but not dec<br>");
echo('b ' . MonthShifter($x,1)->format(('Y-m-d'))."<br>");
echo('c ' . MonthShifter($x,-1)->format(('Y-m-d'))."<br>");

$x = new DateTime('2017-01-15');
echo("<br>" . $x->format('Y-m-d')." middle of the month <br>");
echo('d ' . MonthShifter($x,1)->format(('Y-m-d'))."<br>");
echo('e ' . MonthShifter($x,-1)->format(('Y-m-d'))."<br>");

$x = new DateTime('2017-02-28');
echo("<br>" . $x->format('Y-m-d')." end of Feb<br>");
echo('f ' . MonthShifter($x,1)->format(('Y-m-d'))."<br>");
echo('g ' . MonthShifter($x,-1)->format(('Y-m-d'))."<br>");

$x = new DateTime('2017-01-31');
echo("<br>" .  $x->format('Y-m-d')." end of Jan<br>");
echo('h ' . MonthShifter($x,1)->format(('Y-m-d'))."<br>");
echo('i ' . MonthShifter($x,-1)->format(('Y-m-d'))."<br>");

$x = new DateTime('2017-01-31');
echo("<br>" .  $x->format('Y-m-d')." end of Jan +/- 1 years diff, leap year respected<br>");
echo('j ' . MonthShifter($x,13)->format(('Y-m-d'))."<br>");
echo('k ' . MonthShifter($x,-11)->format(('Y-m-d'))."<br>");

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
k 2016-02-29?>
```
  

#

These functions makes sure that adding months or years always ends up in the month you would expect.  Works for positive and negative values<br><br>

```
<?php
      
       
    $date=new DateTime();
    $date->setDate(2008,2,29);
    
    function addMonths($date,$months){
         
        $init=clone $date;
        $modifier=$months.' months';
        $back_modifier =-$months.' months';
        
        $date->modify($modifier);
        $back_to_init= clone $date;
        $back_to_init->modify($back_modifier);
        
        while($init->format('m')!=$back_to_init->format('m')){
        $date->modify('-1 day')    ;
        $back_to_init= clone $date;
        $back_to_init->modify($back_modifier);    
        }
        
        /*
        if($months<0&amp;&amp;$date->format('m')>$init->format('m'))
        while($date->format('m')-12-$init->format('m')!=$months%12)
        $date->modify('-1 day');
        else
        if($months>0&amp;&amp;$date->format('m')<$init->format('m'))
        while($date->format('m')+12-$init->format('m')!=$months%12)
        $date->modify('-1 day');
        else
        while($date->format('m')-$init->format('m')!=$months%12)
        $date->modify('-1 day');
        */
        
    }
     
    function addYears($date,$years){
        
        $init=clone $date;
        $modifier=$years.' years';
        $date->modify($modifier);
        
        while($date->format('m')!=$init->format('m'))
        $date->modify('-1 day');
        
        
    } 
    
    
    
    addMonths($date,-1);
     addYears($date,3);
    
    
    echo $date->format('F j,Y');
     
 
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.modify.php)

**[To root](/README.md)**