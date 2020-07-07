# random_int





Here is a simple backporting function, it works for PHP &gt;= 5.1



```
<?php
if (!function_exists(&apos;random_int&apos;)) {
&#xA0; &#xA0; function random_int($min, $max) {
&#xA0; &#xA0; &#xA0; &#xA0; if (!function_exists(&apos;mcrypt_create_iv&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;mcrypt must be loaded for random_int to work&apos;, 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; E_USER_WARNING
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return null;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if (!is_int($min) || !is_int($max)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&apos;$min and $max must be integer values&apos;, E_USER_NOTICE);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $min = (int)$min;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $max = (int)$max;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if ($min &gt; $max) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; trigger_error(&apos;$max can\&apos;t be lesser than $min&apos;, E_USER_WARNING);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return null;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $range = $counter = $max - $min;
&#xA0; &#xA0; &#xA0; &#xA0; $bits = 1;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; while ($counter &gt;&gt;= 1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ++$bits;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $bytes = (int)max(ceil($bits/8), 1);
&#xA0; &#xA0; &#xA0; &#xA0; $bitmask = pow(2, $bits) - 1;
 
&#xA0; &#xA0; &#xA0; &#xA0; if ($bitmask &gt;= PHP_INT_MAX) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bitmask = PHP_INT_MAX;
&#xA0; &#xA0; &#xA0; &#xA0; }
 
&#xA0; &#xA0; &#xA0; &#xA0; do {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = hexdec(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; bin2hex(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; mcrypt_create_iv($bytes, MCRYPT_DEV_URANDOM)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ) &amp; $bitmask;
&#xA0; &#xA0; &#xA0; &#xA0; } while ($result &gt; $range);
 
&#xA0; &#xA0; &#xA0; &#xA0; return $result + $min;
&#xA0; &#xA0; }
}
?>
```


Randomness test



```
<?php
$max = 100; // number of random values
$test = 1000000;

$array = array_fill(0, $max, 0);

for ($i = 0; $i &lt; $test; ++$i) {
&#xA0; &#xA0; ++$array[random_int(0, $max-1)];
}

function arrayFormatResult(&amp;$item) {
&#xA0; &#xA0; global $test, $max; // try to avoid this nowdays ;)
&#xA0; &#xA0; 
&#xA0; &#xA0; $perc = ($item/($test/$max))-1;
&#xA0; &#xA0; $item .= &apos; &apos;. number_format($perc, 4, &apos;.&apos;, &apos;&apos;) .&apos;%&apos;;
}

array_walk($array, &apos;arrayFormatResult&apos;);

print_r($array);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.random-int.php)

**[To root](/README.md)**