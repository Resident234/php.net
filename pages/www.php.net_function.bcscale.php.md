# bcscale





Executing bcsacle() will change the scale value of fpm.conf, not only the current process.

  

#



These functions DO NOT round off your values. No arbitrary precision libraries do it this way. It stops calculating after reaching scale of decimal places, which mean that your value is cut off after scale number of digits, not rounded. To do the rounding use something like this:


```
<?php
&#xA0; &#xA0; &#xA0; &#xA0; function bcround($number, $scale=0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fix = &quot;5&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for ($i=0;$i&lt;$scale;$i++) $fix=&quot;0$fix&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $number = bcadd($number, &quot;0.$fix&quot;, $scale+1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return&#xA0; &#xA0; bcdiv($number, &quot;1.0&quot;,&#xA0; &#xA0; $scale);
&#xA0; &#xA0; &#xA0; &#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.bcscale.php)

**[To root](/README.md)**