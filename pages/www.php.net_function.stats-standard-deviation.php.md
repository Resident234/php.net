# stats_standard_deviation





Here&apos;s an implementation that adheres to the PECL source quite strictly. This is useful for people who want to use it but don&apos;t have the extension, as well as for people who want to understand what is going on.



```
<?php

if (!function_exists(&apos;stats_standard_deviation&apos;)) {
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * This user-land implementation follows the implementation quite strictly;
&#xA0; &#xA0;&#xA0; * it does not attempt to improve the code or algorithm in any way. It will
&#xA0; &#xA0;&#xA0; * raise a warning if you have fewer than 2 values in your array, just like
&#xA0; &#xA0;&#xA0; * the extension does (although as an E_USER_WARNING, not E_WARNING).
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * @param array $a 
&#xA0; &#xA0;&#xA0; * @param bool $sample [optional] Defaults to false
&#xA0; &#xA0;&#xA0; * @return float|bool The standard deviation or false on error.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; function stats_standard_deviation(array $a, $sample = false) {
&#xA0; &#xA0; &#xA0; &#xA0; $n = count($a);
&#xA0; &#xA0; &#xA0; &#xA0; if ($n === 0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&quot;The array has zero elements&quot;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if ($sample &amp;&amp; $n === 1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&quot;The array has only 1 element&quot;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $mean = array_sum($a) / $n;
&#xA0; &#xA0; &#xA0; &#xA0; $carry = 0.0;
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($a as $val) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $d = ((double) $val) - $mean;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $carry += $d * $d;
&#xA0; &#xA0; &#xA0; &#xA0; };
&#xA0; &#xA0; &#xA0; &#xA0; if ($sample) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; --$n;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return sqrt($carry / $n);
&#xA0; &#xA0; }
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.stats-standard-deviation.php)

**[To root](/README.md)**