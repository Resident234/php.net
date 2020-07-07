# ceil





I needed this and couldn&apos;t find it so I thought someone else wouldn&apos;t have to look through a bunch of Google results-



```
<?php

// duplicates m$ excel&apos;s ceiling function
if( !function_exists(&apos;ceiling&apos;) )
{
&#xA0; &#xA0; function ceiling($number, $significance = 1)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return ( is_numeric($number) &amp;&amp; is_numeric($significance) ) ? (ceil($number/$significance)*$significance) : false;
&#xA0; &#xA0; }
}

echo ceiling(0, 1000);&#xA0; &#xA0;&#xA0; // 0
echo ceiling(1, 1);&#xA0; &#xA0; &#xA0; &#xA0; // 1000
echo ceiling(1001, 1000);&#xA0; // 2000
echo ceiling(1.27, 0.05);&#xA0; // 1.30

?>
```



  

#



Caution!


```
<?php
$value = 77.4;
echo ceil($value * 100) / 100;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // 77.41 - WRONG!
echo ceil(round($value * 100)) / 100;&#xA0; // 77.4 - OK!


  

#



I couldn&apos;t find any functions to do what ceiling does while still leaving I specified number of decimal places, so I wrote a couple functions myself.&#xA0; round_up is like ceil but allows you to specify a number of decimal places.&#xA0; round_out does the same, but rounds away from zero.



```
<?php
 // round_up:
 // rounds up a float to a specified number of decimal places
 // (basically acts like ceil() but allows for decimal places)
 function round_up ($value, $places=0) {
&#xA0; if ($places &lt; 0) { $places = 0; }
&#xA0; $mult = pow(10, $places);
&#xA0; return ceil($value * $mult) / $mult;
 }

 // round_out:
 // rounds a float away from zero to a specified number of decimal places
 function round_out ($value, $places=0) {
&#xA0; if ($places &lt; 0) { $places = 0; }
&#xA0; $mult = pow(10, $places);
&#xA0; return ($value &gt;= 0 ? ceil($value * $mult):floor($value * $mult)) / $mult;
 }

 echo round_up (56.77001, 2); // displays 56.78
 echo round_up (-0.453001, 4); // displays -0.453
 echo round_out (56.77001, 2); // displays 56.78
 echo round_out (-0.453001, 4); // displays -0.4531
?>
```



  

#



Actual behaviour:
echo ceil(-0.1); //result &quot;-0&quot; but i expect &quot;0&quot;

Workaround:
echo ceil(-0.1)+0; //result &quot;0&quot;

  

#

[Official documentation page](https://www.php.net/manual/en/function.ceil.php)

**[To root](/README.md)**