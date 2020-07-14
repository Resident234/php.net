# floor



I believe this behavior of the floor function was intended.  Note that it says "the next lowest integer".  -1 is "higher" than -1.6.  As in, -1 is logically greater than -1.6.  To go lower the floor function would go to -2 which is logically less than -1.6.<br><br>Floor isn&apos;t trying to give you the number closest to zero, it&apos;s giving you the lowest bounding integer of a float.<br><br>In reply to Glen who commented:<br> Glen<br>01-Dec-2007 04:22<br>

```
<?php
  echo floor(1.6);  // will output "1"
  echo floor(-1.6); // will output "-2"
?>
```


instead use intval (seems to work v5.1.6):



```
<?php
  echo intval(1.6);  // will output "1"
  echo intval(-1.6); // will output "-1"
?>
```
  

#

I use this function to floor with decimals:<br>

```
<?php

function floordec($zahl,$decimals=2){    
     return floor($zahl*pow(10,$decimals))/pow(10,$decimals);
}
?>
```
  

#

A correction to the funcion floor_dec from the user "php is the best".<br>If the number is 0.05999 it returns 0.59 because the zero at left position is deleted.<br>I just added a &apos;1&apos; and after the floor or ceil call remove with a substr.<br>Hope it helps.<br><br>function floor_dec($number,$precision = 2,$separator = &apos;.&apos;) {<br>  $numberpart=explode($separator,$number);<br>  $numberpart[1]=substr_replace($numberpart[1],$separator,$precision,0);<br>  if($numberpart[0]&gt;=0) {<br>    $numberpart[1]=substr(floor(&apos;1&apos;.$numberpart[1]),1);<br>  } else {<br>    $numberpart[1]=substr(ceil(&apos;1&apos;.$numberpart[1]),1);<br>  }<br>  $ceil_number= array($numberpart[0],$numberpart[1]);<br>  return implode($separator,$ceil_number);<br>}  

#

But, if you want the number closest to zero, you could use this:<br>

```
<?php
  if($foo &gt; 0) {
    floor($foo);
  } else {
    ceil($foo);
  }
?>
```
<br><br>-benrr101  

#

Have solved a "price problem":<br><br>

```
<?php
$peny = floor($row-&gt;price*1000) - floor($row-&gt;price)*1000)/10;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.floor.php)

**[To root](/README.md)**