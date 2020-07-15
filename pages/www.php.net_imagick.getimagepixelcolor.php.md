# Imagick::getImagePixelColor



I&apos;m sure there are a lot of people like me who have been wondering, "How you manage to produce a human readable output of this operation?"<br><br>

```
<?php
$image = new Imagick('testimage.jpg');

$x = 1; 
$y = 1;
$pixel = $image->getImagePixelColor($x, $y);
?>
```


If you try to print an output of the $pixel object, you get nothing. You have to use one of the ImagickPixel operations to get back a value.

You can do either of the following:



```
<?php
$colors = $pixel->getColor();
print_r($colors); // produces Array([r]=>255,[g]=>255,[b]=>255,[a]=>1);

$pixel->getColorAsString(); // produces rgb(255,255,255);
?>
```


The place where I was getting hung up was how to get the data that was captured in the Imagick::getImagePixelColor operation into an ImagickPixel object. I was trying to find ways of passing the value to a newly instantiated ImagickPixel object. Well, it appears that once you've captured your color data using Imagick::getImagePixelColor, what's returned IS an ImagickPixel object!

As a further note, you do not need to convert this to a human readable format if you just want to take a color sample at a single point on your image to plug into another operation. 

For example, if you wanted to perform a flood fill effect on a certain color you could plug in the instance of the ImagickPixel object directly. 

The following fill perform a flood fill effect at coordinates 1,1 on your image using Green as the fill color and the color sampled at 1,1 as the target color to fill.



```
<?php
$hexcolor = '#00ff00';
$fuzz = '4000';
$x = 1;
$y = 1;
$pixel = $image->getImagePixelColor($x, $y);
$image->floodfillPaintImage($hexcolor, $fuzz, $pixel, $x, $y, false);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.getimagepixelcolor.php)

**[To root](/README.md)**