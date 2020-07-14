# number_format



It&apos;s not explicitly documented; number_format also rounds:<br><br>

```
<?php
$numbers = array(0.001, 0.002, 0.003, 0.004, 0.005, 0.006, 0.007, 0.008, 0.009);
foreach ($numbers as $number)
    print $number."-&gt;".number_format($number, 2, &apos;.&apos;, &apos;,&apos;)."&lt;br&gt;";
?>
```
<br><br>0.001-&gt;0.00<br>0.002-&gt;0.00<br>0.003-&gt;0.00<br>0.004-&gt;0.00<br>0.005-&gt;0.01<br>0.006-&gt;0.01<br>0.007-&gt;0.01<br>0.008-&gt;0.01<br>0.009-&gt;0.01  

#

I ran across an issue where I wanted to keep the entered precision of a real value, without arbitrarily rounding off what the user had submitted.<br><br>I figured it out with a quick explode on the number before formatting. I could then format either side of the decimal.<br><br>

```
<?php
      function number_format_unlimited_precision($number,$decimal = &apos;.&apos;)
      {
           $broken_number = explode($decimal,$number);
           return number_format($broken_number[0]).$decimal.$broken_number[1];
      }
?>
```
  

#

Outputs a human readable number.<br><br>

```
<?php
    #    Output easy-to-read numbers
    #    by james at bandit.co.nz
    function bd_nice_number($n) {
        // first strip any formatting;
        $n = (0+str_replace(",","",$n));
        
        // is this a number?
        if(!is_numeric($n)) return false;
        
        // now filter it;
        if($n&gt;1000000000000) return round(($n/1000000000000),1).&apos; trillion&apos;;
        else if($n&gt;1000000000) return round(($n/1000000000),1).&apos; billion&apos;;
        else if($n&gt;1000000) return round(($n/1000000),1).&apos; million&apos;;
        else if($n&gt;1000) return round(($n/1000),1).&apos; thousand&apos;;
        
        return number_format($n);
    }
?>
```
<br><br>Outputs:<br><br>247,704,360 -&gt; 247.7 million<br>866,965,260,000 -&gt; 867 billion  

#

For Zero fill - just use the sprintf() function<br><br>$pr_id = 1;<br>$pr_id = sprintf("%03d", $pr_id);<br>echo $pr_id;<br><br>//outputs 001<br>-----------------<br><br>$pr_id = 10;<br>$pr_id = sprintf("%03d", $pr_id);<br>echo $pr_id;<br><br>//outputs 010<br>-----------------<br><br>You can change %03d to %04d, etc.  

#

Just an observation:<br>The number_format rounds the value of the variable.<br><br>$val1 = 1.233;<br>$val2 = 1.235;<br>$val3 = 1.237;<br><br>echo number_format($val1,2,",","."); // returns: 1,23<br>echo number_format($val2,2,",","."); // returns: 1,24<br>echo number_format($val3,2,",","."); // returns: 1,24  

#

[Official documentation page](https://www.php.net/manual/en/function.number-format.php)

**[To root](/README.md)**