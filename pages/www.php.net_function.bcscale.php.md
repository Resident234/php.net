# bcscale



Executing bcsacle() will change the scale value of fpm.conf, not only the current process.  

#

These functions DO NOT round off your values. No arbitrary precision libraries do it this way. It stops calculating after reaching scale of decimal places, which mean that your value is cut off after scale number of digits, not rounded. To do the rounding use something like this:<br>

```
<?php
        function bcround($number, $scale=0) {
                $fix = "5";
                for ($i=0;$i<$scale;$i++) $fix="0$fix";
                $number = bcadd($number, "0.$fix", $scale+1);
                return    bcdiv($number, "1.0",    $scale);
        }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.bcscale.php)

**[To root](/README.md)**