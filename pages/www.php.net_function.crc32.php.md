# crc32



This function returns an unsigned integer from a 64-bit Linux platform.  It does return the signed integer from other 32-bit platforms even a 64-bit Windows one.<br><br>The reason is because the two constants PHP_INT_SIZE and PHP_INT_MAX have different values on the 64-bit Linux platform.<br><br>I&apos;ve created a work-around function to handle this situation.<br><br>

```
<?php
function get_signed_int($in) {
    $int_max = pow(2, 31)-1;
    if ($in > $int_max){
        $out = $in - $int_max * 2 - 2;
    }
    else {
        $out = $in;
    }
    return $out;
}
?>
```
<br><br>Hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/function.crc32.php)

**[To root](/README.md)**