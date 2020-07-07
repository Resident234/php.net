# round





In my opinion this function lacks two flags:

- PHP_ROUND_UP - Always round up.
- PHP_ROUND_DOWN - Always round down.

In accounting, it&apos;s often necessary to always round up, or down to a precision of thousandths.



```
<?php
function round_up($number, $precision = 2)
{
&#xA0; &#xA0; $fig = (int) str_pad(&apos;1&apos;, $precision, &apos;0&apos;);
&#xA0; &#xA0; return (ceil($number * $fig) / $fig);
}

function round_down($number, $precision = 2)
{
&#xA0; &#xA0; $fig = (int) str_pad(&apos;1&apos;, $precision, &apos;0&apos;);
&#xA0; &#xA0; return (floor($number * $fig) / $fig);
}
?>
```



  

#



As PHP doesn&apos;t have a a native number truncate function, this is my solution - a function that can be usefull if you need truncate instead round a number.



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
function truncate($val, $f=&quot;0&quot;)
{
&#xA0; &#xA0; if(($p = strpos($val, &apos;.&apos;)) !== false) {
&#xA0; &#xA0; &#xA0; &#xA0; $val = floatval(substr($val, 0, $p + 1 + $f));
&#xA0; &#xA0; }
&#xA0; &#xA0; return $val;
}
?>
```


Originally posted in http://stackoverflow.com/a/12710283/1596489

  

#



If you have negative zero and you need return positive number simple add +0:

$number = -2.38419e-07;
var_dump(round($number,1));//float(-0)
var_dump(round($number,1) + 0);//float(0)

  

#



I discovered that under some conditions you can get rounding errors with round when converting the number to a string afterwards.

To fix this I swapped round() for number_format().

Unfortunately i cant give an example (because the number cant be represented as a string !)

essentially I had round(0.688888889,2);

which would stay as 0.68888889 when printed as a string.

But using number_format it correctly became 0.69.

  

#



If you&apos;d only want to round for displaying variables (not for calculating on the rounded result) then you should use printf with the float:





```
<?php printf (&quot;%6.2f&quot;,3.39532); ?>
```




This returns: 3.40 .

  

#



Here is function that rounds to a specified increment, but always up. I had to use it for price adjustment that always went up to $5 increments.





```
<?php&#xA0; 

function roundUpTo($number, $increments) {

&#xA0; &#xA0; $increments = 1 / $increments;

&#xA0; &#xA0; return (ceil($number * $increments) / $increments);

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.round.php)

**[To root](/README.md)**