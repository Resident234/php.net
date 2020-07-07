# floor





I believe this behavior of the floor function was intended.&#xA0; Note that it says &quot;the next lowest integer&quot;.&#xA0; -1 is &quot;higher&quot; than -1.6.&#xA0; As in, -1 is logically greater than -1.6.&#xA0; To go lower the floor function would go to -2 which is logically less than -1.6.

Floor isn&apos;t trying to give you the number closest to zero, it&apos;s giving you the lowest bounding integer of a float.

In reply to Glen who commented:
 Glen
01-Dec-2007 04:22


```
<?php
&#xA0; echo floor(1.6);&#xA0; // will output &quot;1&quot;
&#xA0; echo floor(-1.6); // will output &quot;-2&quot;
?>
```


instead use intval (seems to work v5.1.6):



```
<?php
&#xA0; echo intval(1.6);&#xA0; // will output &quot;1&quot;
&#xA0; echo intval(-1.6); // will output &quot;-1&quot;
?>
```



  

#



I use this function to floor with decimals:


```
<?php

function floordec($zahl,$decimals=2){&#xA0; &#xA0; 
&#xA0; &#xA0;&#xA0; return floor($zahl*pow(10,$decimals))/pow(10,$decimals);
}
?>
```



  

#



A correction to the funcion floor_dec from the user &quot;php is the best&quot;.
If the number is 0.05999 it returns 0.59 because the zero at left position is deleted.
I just added a &apos;1&apos; and after the floor or ceil call remove with a substr.
Hope it helps.

function floor_dec($number,$precision = 2,$separator = &apos;.&apos;) {
&#xA0; $numberpart=explode($separator,$number);
&#xA0; $numberpart[1]=substr_replace($numberpart[1],$separator,$precision,0);
&#xA0; if($numberpart[0]&gt;=0) {
&#xA0; &#xA0; $numberpart[1]=substr(floor(&apos;1&apos;.$numberpart[1]),1);
&#xA0; } else {
&#xA0; &#xA0; $numberpart[1]=substr(ceil(&apos;1&apos;.$numberpart[1]),1);
&#xA0; }
&#xA0; $ceil_number= array($numberpart[0],$numberpart[1]);
&#xA0; return implode($separator,$ceil_number);
}

  

#



But, if you want the number closest to zero, you could use this:


```
<?php
&#xA0; if($foo &gt; 0) {
&#xA0; &#xA0; floor($foo);
&#xA0; } else {
&#xA0; &#xA0; ceil($foo);
&#xA0; }
?>
```


-benrr101

  

#



Have solved a &quot;price problem&quot;:





```
<?php

$peny = floor($row-&gt;price*1000) - floor($row-&gt;price)*1000)/10;

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.floor.php)

**[To root](/README.md)**