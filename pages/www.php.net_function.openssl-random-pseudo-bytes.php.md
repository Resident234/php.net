# openssl_random_pseudo_bytes





Here&apos;s an example to show the distribution of random numbers as an image. Credit to Hayley Watson at the mt_rand page for the original comparison between rand and mt_rand.

rand is red, mt_rand is green and openssl_random_pseudo_bytes is blue.

NOTE: This is only a basic representation of the distribution of the data. Has nothing to do with the strength of the algorithms or their reliability.



```
<?php
header(&quot;Content-type: image/png&quot;);
$sizex=800;
$sizey=800;

$img = imagecreatetruecolor(3 * $sizex,$sizey);
$r = imagecolorallocate($img,255, 0, 0);
$g = imagecolorallocate($img,0, 255, 0);
$b = imagecolorallocate($img,0, 0, 255);
imagefilledrectangle($img, 0, 0, 3 * $sizex, $sizey, imagecolorallocate($img, 255, 255, 255));

$p = 0;
for($i=0; $i &lt; 100000; $i++) {
&#xA0; &#xA0; $np = rand(0,$sizex);
&#xA0; &#xA0; imagesetpixel($img, $p, $np, $r);
&#xA0; &#xA0; $p = $np;
}

$p = 0;
for($i=0; $i &lt; 100000; $i++) {
&#xA0; &#xA0; $np = mt_rand(0,$sizex);
&#xA0; &#xA0; imagesetpixel($img, $p + $sizex, $np, $g);
&#xA0; &#xA0; $p = $np;
}

$p = 0;
for($i=0; $i &lt; 100000; $i++) {
&#xA0; &#xA0; $np = floor($sizex*(hexdec(bin2hex(openssl_random_pseudo_bytes(4)))/0xffffffff));
&#xA0; &#xA0; imagesetpixel($img, $p + (2*$sizex), $np, $b);
&#xA0; &#xA0; $p = $np;
}

imagepng($img);
imagedestroy($img);
?>
```



  

#



[Editor&apos;s note: the bug has been fixed as of PHP 5.4.44, 5.5.28 and PHP 5.6.12]



Until PHP 5.6 openssl_random_pseudo_bytes() did NOT use a &quot;cryptographically strong algorithm&quot;! 

See bug report https://bugs.php.net/bug.php?id=70014 and the corresponding source code at https://github.com/php/php-src/blob/php-5.6.10/ext/openssl/openssl.c#L5408

  

#



Another replacement for rand() using OpenSSL.

Note that a solution where the result is truncated using the modulo operator ( % ) is not cryptographically secure, as the generated numbers are not equally distributed, i.e. some numbers may occur more often than others.

A better solution than using the modulo operator is to drop the result if it is too large and generate a new one.



```
<?php
function crypto_rand_secure($min, $max) {
&#xA0; &#xA0; &#xA0; &#xA0; $range = $max - $min;
&#xA0; &#xA0; &#xA0; &#xA0; if ($range == 0) return $min; // not so random...
&#xA0; &#xA0; &#xA0; &#xA0; $log = log($range, 2);
&#xA0; &#xA0; &#xA0; &#xA0; $bytes = (int) ($log / 8) + 1; // length in bytes
&#xA0; &#xA0; &#xA0; &#xA0; $bits = (int) $log + 1; // length in bits
&#xA0; &#xA0; &#xA0; &#xA0; $filter = (int) (1 &lt;&lt; $bits) - 1; // set all lower bits to 1
&#xA0; &#xA0; &#xA0; &#xA0; do {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rnd = hexdec(bin2hex(openssl_random_pseudo_bytes($bytes, $s)));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rnd = $rnd &amp; $filter; // discard irrelevant bits
&#xA0; &#xA0; &#xA0; &#xA0; } while ($rnd &gt;= $range);
&#xA0; &#xA0; &#xA0; &#xA0; return $min + $rnd;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-random-pseudo-bytes.php)

**[To root](/README.md)**