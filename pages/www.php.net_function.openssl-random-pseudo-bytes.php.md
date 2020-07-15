# openssl_random_pseudo_bytes



Here&apos;s an example to show the distribution of random numbers as an image. Credit to Hayley Watson at the mt_rand page for the original comparison between rand and mt_rand.<br><br>rand is red, mt_rand is green and openssl_random_pseudo_bytes is blue.<br><br>NOTE: This is only a basic representation of the distribution of the data. Has nothing to do with the strength of the algorithms or their reliability.<br><br>

```
<?php
header("Content-type: image/png");
$sizex=800;
$sizey=800;

$img = imagecreatetruecolor(3 * $sizex,$sizey);
$r = imagecolorallocate($img,255, 0, 0);
$g = imagecolorallocate($img,0, 255, 0);
$b = imagecolorallocate($img,0, 0, 255);
imagefilledrectangle($img, 0, 0, 3 * $sizex, $sizey, imagecolorallocate($img, 255, 255, 255));

$p = 0;
for($i=0; $i &lt; 100000; $i++) {
    $np = rand(0,$sizex);
    imagesetpixel($img, $p, $np, $r);
    $p = $np;
}

$p = 0;
for($i=0; $i &lt; 100000; $i++) {
    $np = mt_rand(0,$sizex);
    imagesetpixel($img, $p + $sizex, $np, $g);
    $p = $np;
}

$p = 0;
for($i=0; $i &lt; 100000; $i++) {
    $np = floor($sizex*(hexdec(bin2hex(openssl_random_pseudo_bytes(4)))/0xffffffff));
    imagesetpixel($img, $p + (2*$sizex), $np, $b);
    $p = $np;
}

imagepng($img);
imagedestroy($img);
?>
```
  

#

[Editor&apos;s note: the bug has been fixed as of PHP 5.4.44, 5.5.28 and PHP 5.6.12]<br><br>Until PHP 5.6 openssl_random_pseudo_bytes() did NOT use a "cryptographically strong algorithm"! <br>See bug report https://bugs.php.net/bug.php?id=70014 and the corresponding source code at https://github.com/php/?>
```
src/blob/?>
```
5.6.10/ext/openssl/openssl.c#L5408  

#

Another replacement for rand() using OpenSSL.<br><br>Note that a solution where the result is truncated using the modulo operator ( % ) is not cryptographically secure, as the generated numbers are not equally distributed, i.e. some numbers may occur more often than others.<br><br>A better solution than using the modulo operator is to drop the result if it is too large and generate a new one.<br><br>

```
<?php
function crypto_rand_secure($min, $max) {
        $range = $max - $min;
        if ($range == 0) return $min; // not so random...
        $log = log($range, 2);
        $bytes = (int) ($log / 8) + 1; // length in bytes
        $bits = (int) $log + 1; // length in bits
        $filter = (int) (1 &lt;&lt; $bits) - 1; // set all lower bits to 1
        do {
            $rnd = hexdec(bin2hex(openssl_random_pseudo_bytes($bytes, $s)));
            $rnd = $rnd &amp; $filter; // discard irrelevant bits
        } while ($rnd &gt;= $range);
        return $min + $rnd;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.openssl-random-pseudo-bytes.php)

**[To root](/README.md)**