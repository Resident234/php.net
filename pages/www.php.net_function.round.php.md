# round



In my opinion this function lacks two flags:<br><br>- PHP_ROUND_UP - Always round up.<br>- PHP_ROUND_DOWN - Always round down.<br><br>In accounting, it&apos;s often necessary to always round up, or down to a precision of thousandths.<br><br>

```
<?php
function round_up($number, $precision = 2)
{
    $fig = (int) str_pad(&apos;1&apos;, $precision, &apos;0&apos;);
    return (ceil($number * $fig) / $fig);
}

function round_down($number, $precision = 2)
{
    $fig = (int) str_pad(&apos;1&apos;, $precision, &apos;0&apos;);
    return (floor($number * $fig) / $fig);
}
?>
```
  

#

As PHP doesn&apos;t have a a native number truncate function, this is my solution - a function that can be usefull if you need truncate instead round a number.<br><br>

```
<?php
/**
 * Truncate a float number, example: &lt;code&gt;truncate(-1.49999, 2); // returns -1.49
 * truncate(.49999, 3); // returns 0.499
 * &lt;/code&gt;
 * @param float $val Float number to be truncate
 * @param int f Number of precision
 * @return float
 */
function truncate($val, $f="0")
{
    if(($p = strpos($val, &apos;.&apos;)) !== false) {
        $val = floatval(substr($val, 0, $p + 1 + $f));
    }
    return $val;
}
?>
```
<br><br>Originally posted in http://stackoverflow.com/a/12710283/1596489  

#

If you have negative zero and you need return positive number simple add +0:<br><br>$number = -2.38419e-07;<br>var_dump(round($number,1));//float(-0)<br>var_dump(round($number,1) + 0);//float(0)  

#

I discovered that under some conditions you can get rounding errors with round when converting the number to a string afterwards.<br><br>To fix this I swapped round() for number_format().<br><br>Unfortunately i cant give an example (because the number cant be represented as a string !)<br><br>essentially I had round(0.688888889,2);<br><br>which would stay as 0.68888889 when printed as a string.<br><br>But using number_format it correctly became 0.69.  

#

If you&apos;d only want to round for displaying variables (not for calculating on the rounded result) then you should use printf with the float:<br><br>

```
<?php printf ("%6.2f",3.39532); ?>
```
<br><br>This returns: 3.40 .  

#

Here is function that rounds to a specified increment, but always up. I had to use it for price adjustment that always went up to $5 increments.<br><br>

```
<?php  
function roundUpTo($number, $increments) {
    $increments = 1 / $increments;
    return (ceil($number * $increments) / $increments);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.round.php)

**[To root](/README.md)**