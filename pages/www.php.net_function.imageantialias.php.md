# imageantialias



If you can&apos;t be bothered creating (or searching for) a full screen antialias function.<br>You can actually cheat (well a bit of a dirty inefficient hack really!!) <br>and perform a fake antialias on an image by using &apos;imagecopyresampled&apos;...<br><br>first create your source image twice the size of what you really want.<br><br>Then use &apos;imagecopyresampled&apos; to shrink it to half the size, the function <br>automatically interpolates pixels to create an antialias effect!<br><br>I&apos;ve used this in a pie chart function and it works brilliantly,<br>not as slow as I thought it might be!<br><br>the rough code below should give you the idea...<br><br>

```
<?php
$realWidth = 500;
$realHeight = 500;
$srcWidth = $realWidth * 2;
$srcHeight = $realHeight * 2;

// create the larger source image
$srcImage = imagecreatetruecolor($srcWidth,$srcHeight);

// create the real/final image
$destImage = imagecreatetruecolor($realWidth,$realHeight);

// now do whatever you want to draw in the source image
// blah....

// now the picture is finished, do the shrink...
imagecopyresampled($destImage,$srcImage,0,0,0,0,
$realWidth,$realHeight,$srcWidth,$srcHeight);

// now just do whatever you want with &apos;$destImage&apos; (e.g. display or output to file!)
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imageantialias.php)

**[To root](/README.md)**