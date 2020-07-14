# random_int



Here is a simple backporting function, it works for PHP &gt;= 5.1<br><br>

```
<?php
if (!function_exists(&apos;random_int&apos;)) {
    function random_int($min, $max) {
        if (!function_exists(&apos;mcrypt_create_iv&apos;)) {
            trigger_error(
                &apos;mcrypt must be loaded for random_int to work&apos;, 
                E_USER_WARNING
            );
            return null;
        }
        
        if (!is_int($min) || !is_int($max)) {
            trigger_error(&apos;$min and $max must be integer values&apos;, E_USER_NOTICE);
            $min = (int)$min;
            $max = (int)$max;
        }
        
        if ($min &gt; $max) {
            trigger_error(&apos;$max can\&apos;t be lesser than $min&apos;, E_USER_WARNING);
            return null;
        }
        
        $range = $counter = $max - $min;
        $bits = 1;
        
        while ($counter &gt;&gt;= 1) {
            ++$bits;
        }
        
        $bytes = (int)max(ceil($bits/8), 1);
        $bitmask = pow(2, $bits) - 1;
 
        if ($bitmask &gt;= PHP_INT_MAX) {
            $bitmask = PHP_INT_MAX;
        }
 
        do {
            $result = hexdec(
                bin2hex(
                    mcrypt_create_iv($bytes, MCRYPT_DEV_URANDOM)
                )
            ) &amp; $bitmask;
        } while ($result &gt; $range);
 
        return $result + $min;
    }
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
    ++$array[random_int(0, $max-1)];
}

function arrayFormatResult(&amp;$item) {
    global $test, $max; // try to avoid this nowdays ;)
    
    $perc = ($item/($test/$max))-1;
    $item .= &apos; &apos;. number_format($perc, 4, &apos;.&apos;, &apos;&apos;) .&apos;%&apos;;
}

array_walk($array, &apos;arrayFormatResult&apos;);

print_r($array);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.random-int.php)

**[To root](/README.md)**