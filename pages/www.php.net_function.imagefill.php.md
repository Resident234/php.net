# imagefill





Creating a new true-color image, filling it with a transparent color, and saving it as a PNG image can be accomplished with the following:



```
<?php

$new&#xA0;&#xA0; = imagecreatetruecolor(320, 320);
$color = imagecolorallocatealpha($new, 0, 0, 0, 127);
imagefill($new, 0, 0, $color);
imagesavealpha($new, TRUE); // it took me a good 10 minutes to figure this part out
imagepng($new);

?>
```


The image needs to be created with imagecreatetruecolor(), you must use imagefill() instead of imagefilledrectange(), and you need to call imagesavealpha(). No other combination of functions calls seems to produce the intended results.

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagefill.php)

**[To root](/README.md)**