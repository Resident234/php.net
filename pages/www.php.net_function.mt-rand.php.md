# mt_rand





i wanted to spot out the big difference between rand and mt_rand when producing images using randomness as noise.



for example this is a comparation between rand and mt_rand on a 400x400 pixel png: http://oi43.tinypic.com/vwtppl.jpg



code:



```
<?php

header(&quot;Content-type: image/png&quot;);

$sizex=800;

$sizey=400;



$img = imagecreatetruecolor($sizex,$sizey);

$ink = imagecolorallocate($img,255,255,255);



for($i=0;$i&lt;$sizex/2;$i++) {

&#xA0; for($j=0;$j&lt;$sizey;$j++) {

&#xA0; imagesetpixel($img, rand(1,$sizex/2), rand(1,$sizey), $ink);

&#xA0; }

}

 

for($i=$sizex/2;$i&lt;$sizex;$i++) {

&#xA0; for($j=0;$j&lt;$sizey;$j++) {

&#xA0; imagesetpixel($img, mt_rand($sizex/2,$sizex), mt_rand(1,$sizey), $ink);

&#xA0; }

}



imagepng($img);

imagedestroy($img);

?>
```




the differences reduce when reducing the pixels of the image.. infact for a 100x100 pixel image the noise produced from the rand function is much more realistic than how it is for a 400x400 image: http://oi39.tinypic.com/5k0row.jpg



(rand is on the left, mt_rand on the right)

  

#



To quickly build a human-readable random string for a captcha per example :



```
<?php

function random($length = 8)
{&#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; $chars = &apos;bcdfghjklmnprstvwxzaeiou&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; for ($p = 0; $p &lt; $length; $p++)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $result .= ($p%2) ? $chars[mt_rand(19, 23)] : $chars[mt_rand(0, 18)];
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; return $result;
}

?>
```


Note that I have removed q and y from $chars to avoid readability problems.

  

#

[Official documentation page](https://www.php.net/manual/en/function.mt-rand.php)

**[To root](/README.md)**