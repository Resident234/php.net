# ceil



I needed this and couldn&apos;t find it so I thought someone else wouldn&apos;t have to look through a bunch of Google results-<br><br>

```
<?php

// duplicates m$ excel's ceiling function
if( !function_exists('ceiling') )
{
    function ceiling($number, $significance = 1)
    {
        return ( is_numeric($number) &amp;&amp; is_numeric($significance) ) ? (ceil($number/$significance)*$significance) : false;
    }
}

echo ceiling(0, 1000);     // 0
echo ceiling(1, 1);        // 1000
echo ceiling(1001, 1000);  // 2000
echo ceiling(1.27, 0.05);  // 1.30

?>
```
  

#

Caution!<br>

```
<?php
$value = 77.4;
echo ceil($value * 100) / 100;         // 77.41 - WRONG!
echo ceil(round($value * 100)) / 100;  // 77.4 - OK!?>
```
  

#

I couldn&apos;t find any functions to do what ceiling does while still leaving I specified number of decimal places, so I wrote a couple functions myself.  round_up is like ceil but allows you to specify a number of decimal places.  round_out does the same, but rounds away from zero.<br><br>

```
<?php
 // round_up:
 // rounds up a float to a specified number of decimal places
 // (basically acts like ceil() but allows for decimal places)
 function round_up ($value, $places=0) {
  if ($places < 0) { $places = 0; }
  $mult = pow(10, $places);
  return ceil($value * $mult) / $mult;
 }

 // round_out:
 // rounds a float away from zero to a specified number of decimal places
 function round_out ($value, $places=0) {
  if ($places < 0) { $places = 0; }
  $mult = pow(10, $places);
  return ($value >= 0 ? ceil($value * $mult):floor($value * $mult)) / $mult;
 }

 echo round_up (56.77001, 2); // displays 56.78
 echo round_up (-0.453001, 4); // displays -0.453
 echo round_out (56.77001, 2); // displays 56.78
 echo round_out (-0.453001, 4); // displays -0.4531
?>
```
  

#

Actual behaviour:<br>echo ceil(-0.1); //result "-0" but i expect "0"<br><br>Workaround:<br>echo ceil(-0.1)+0; //result "0"  

#

[Official documentation page](https://www.php.net/manual/en/function.ceil.php)

**[To root](/README.md)**