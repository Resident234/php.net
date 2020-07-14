# mt_rand



i wanted to spot out the big difference between rand and mt_rand when producing images using randomness as noise.<br><br>for example this is a comparation between rand and mt_rand on a 400x400 pixel png: http://oi43.tinypic.com/vwtppl.jpg<br><br>code:<br>

```
<?php
header("Content-type: image/png");
$sizex=800;
$sizey=400;

$img = imagecreatetruecolor($sizex,$sizey);
$ink = imagecolorallocate($img,255,255,255);

for($i=0;$i&lt;$sizex/2;$i++) {
  for($j=0;$j&lt;$sizey;$j++) {
  imagesetpixel($img, rand(1,$sizex/2), rand(1,$sizey), $ink);
  }
}
 
for($i=$sizex/2;$i&lt;$sizex;$i++) {
  for($j=0;$j&lt;$sizey;$j++) {
  imagesetpixel($img, mt_rand($sizex/2,$sizex), mt_rand(1,$sizey), $ink);
  }
}

imagepng($img);
imagedestroy($img);
?>
```
<br><br>the differences reduce when reducing the pixels of the image.. infact for a 100x100 pixel image the noise produced from the rand function is much more realistic than how it is for a 400x400 image: http://oi39.tinypic.com/5k0row.jpg<br><br>(rand is on the left, mt_rand on the right)  

#

To quickly build a human-readable random string for a captcha per example :<br><br>

```
<?php

function random($length = 8)
{      
    $chars = &apos;bcdfghjklmnprstvwxzaeiou&apos;;
    
    for ($p = 0; $p &lt; $length; $p++)
    {
        $result .= ($p%2) ? $chars[mt_rand(19, 23)] : $chars[mt_rand(0, 18)];
    }
    
    return $result;
}

?>
```
<br><br>Note that I have removed q and y from $chars to avoid readability problems.  

#

[Official documentation page](https://www.php.net/manual/en/function.mt-rand.php)

**[To root](/README.md)**