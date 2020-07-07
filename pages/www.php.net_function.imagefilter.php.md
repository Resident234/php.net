# imagefilter





The documentation misses the exact meaning and valid ranges of the arguments for ImageFilter(). According to the 5.2.0 sources the arguments are:
IMG_FILTER_BRIGHTNESS
-255 = min brightness, 0 = no change, +255 = max brightness

IMG_FILTER_CONTRAST
-100 = max contrast, 0 = no change, +100 = min contrast (note the direction!)

IMG_FILTER_COLORIZE
Adds (subtracts) specified RGB values to each pixel. The valid range for each color is -255...+255, not 0...255. The correct order is red, green, blue.
-255 = min, 0 = no change, +255 = max
This has not much to do with IMG_FILTER_GRAYSCALE.

IMG_FILTER_SMOOTH
Applies a 9-cell convolution matrix where center pixel has the weight arg1 and others weight of 1.0. The result is normalized by dividing the sum with arg1 + 8.0 (sum of the matrix).
any float is accepted, large value (in practice: 2048 or more) = no change

ImageFilter seem to return false if the argument(s) are out of range for the chosen filter.

  

#



I needed an especially strong blur effect today and had a hard time achieving adequate results with the built-in IMG_FILTER_GAUSSIAN_BLUR filter. In order to achieve the strength of the blur I required I had to repeat the filter up to&#xA0; 100 times, which took way too long to be acceptable.

After a bit of searching, I found this answer to be quite a good solution to this problem: http://stackoverflow.com/a/20264482

Based on that technique, I wrote the following generic function to achieve a very strong blur in a reasonable amount of processing:



```
<?php 

/**
 * Strong Blur
 *
 * @param resource $gdImageResource 
 * @param int $blurFactor optional 
 *&#xA0; This is the strength of the blur
 *&#xA0; 0 = no blur, 3 = default, anything over 5 is extremely blurred
 * @return GD image resource
 * @author Martijn Frazer, idea based on http://stackoverflow.com/a/20264482
 */
function blur($gdImageResource, $blurFactor = 3)
{
&#xA0; // blurFactor has to be an integer
&#xA0; $blurFactor = round($blurFactor);
&#xA0; 
&#xA0; $originalWidth = imagesx($gdImageResource);
&#xA0; $originalHeight = imagesy($gdImageResource);

&#xA0; $smallestWidth = ceil($originalWidth * pow(0.5, $blurFactor));
&#xA0; $smallestHeight = ceil($originalHeight * pow(0.5, $blurFactor));

&#xA0; // for the first run, the previous image is the original input
&#xA0; $prevImage = $gdImageResource;
&#xA0; $prevWidth = $originalWidth;
&#xA0; $prevHeight = $originalHeight;

&#xA0; // scale way down and gradually scale back up, blurring all the way
&#xA0; for($i = 0; $i &lt; $blurFactor; $i += 1)
&#xA0; {&#xA0; &#xA0; 
&#xA0; &#xA0; // determine dimensions of next image
&#xA0; &#xA0; $nextWidth = $smallestWidth * pow(2, $i);
&#xA0; &#xA0; $nextHeight = $smallestHeight * pow(2, $i);

&#xA0; &#xA0; // resize previous image to next size
&#xA0; &#xA0; $nextImage = imagecreatetruecolor($nextWidth, $nextHeight);
&#xA0; &#xA0; imagecopyresized($nextImage, $prevImage, 0, 0, 0, 0, 
&#xA0; &#xA0; &#xA0; $nextWidth, $nextHeight, $prevWidth, $prevHeight);

&#xA0; &#xA0; // apply blur filter
&#xA0; &#xA0; imagefilter($nextImage, IMG_FILTER_GAUSSIAN_BLUR);

&#xA0; &#xA0; // now the new image becomes the previous image for the next step
&#xA0; &#xA0; $prevImage = $nextImage;
&#xA0; &#xA0; $prevWidth = $nextWidth;
&#xA0; &#xA0; &#xA0; $prevHeight = $nextHeight;
&#xA0; }

&#xA0; // scale back to original size and blur one more time
&#xA0; imagecopyresized($gdImageResource, $nextImage, 
&#xA0; &#xA0; 0, 0, 0, 0, $originalWidth, $originalHeight, $nextWidth, $nextHeight);
&#xA0; imagefilter($gdImageResource, IMG_FILTER_GAUSSIAN_BLUR);

&#xA0; // clean up
&#xA0; imagedestroy($prevImage);

&#xA0; // return result
&#xA0; return $gdImageResource;
}
?>
```



  

#



Here is an alternative to IMG_FILTER_COLORIZE filter, but taking the alpha parameter of each pixel in account.





```
<?php

function rgba_colorize($img, $color)

{

&#xA0; &#xA0; imagesavealpha($img, true);

&#xA0; &#xA0; imagealphablending($img, true);



&#xA0; &#xA0; $img_x = imagesx($img);

&#xA0; &#xA0; $img_y = imagesy($img);

&#xA0; &#xA0; for ($x = 0; $x &lt; $img_x; ++$x)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; for ($y = 0; $y &lt; $img_y; ++$y)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rgba = imagecolorsforindex($img, imagecolorat($img, $x, $y));

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color_alpha = imagecolorallocatealpha($img, $color[0], $color[1], $color[2], $rgba[&apos;alpha&apos;]);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; imagesetpixel($img, $x, $y, $color_alpha);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; imagecolordeallocate($img, $color_alpha);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagefilter.php)

**[To root](/README.md)**