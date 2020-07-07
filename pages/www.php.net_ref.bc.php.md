# BC Math Functions





Please be aware not to use/have spaces in your strings. It took me a while to find the error in some advanced calculations!



```
<?php
echo bcadd(&quot;1&quot;, &quot;2&quot;); // 3
echo bcadd(&quot;1&quot;, &quot;2 &quot;); // 1
echo bcadd(&quot;1&quot;, &quot; 2&quot;); // 1
?>
```



  

#



Here are some useful functions to convert large hex numbers from and to large decimal ones :



```
<?php
&#xA0; &#xA0; public static function bchexdec($hex) {
&#xA0; &#xA0; &#xA0; &#xA0; if(strlen($hex) == 1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return hexdec($hex);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $remain = substr($hex, 0, -1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $last = substr($hex, -1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return bcadd(bcmul(16, bchexdec($remain)), hexdec($last));
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public static function bcdechex($dec) {
&#xA0; &#xA0; &#xA0; &#xA0; $last = bcmod($dec, 16);
&#xA0; &#xA0; &#xA0; &#xA0; $remain = bcdiv(bcsub($dec, $last), 16);

&#xA0; &#xA0; &#xA0; &#xA0; if($remain == 0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return dechex($last);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return bcdechex($remain).dechex($last);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }


  

#

[Official documentation page](https://www.php.net/manual/en/ref.bc.php)

**[To root](/README.md)**