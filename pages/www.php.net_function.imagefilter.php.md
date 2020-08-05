# imagefilter



The documentation misses the exact meaning and valid ranges of the arguments for ImageFilter(). According to the 5.2.0 sources the arguments are:<br>IMG_FILTER_BRIGHTNESS<br>-255 = min brightness, 0 = no change, +255 = max brightness<br><br>IMG_FILTER_CONTRAST<br>-100 = max contrast, 0 = no change, +100 = min contrast (note the direction!)<br><br>IMG_FILTER_COLORIZE<br>Adds (subtracts) specified RGB values to each pixel. The valid range for each color is -255...+255, not 0...255. The correct order is red, green, blue.<br>-255 = min, 0 = no change, +255 = max<br>This has not much to do with IMG_FILTER_GRAYSCALE.<br><br>IMG_FILTER_SMOOTH<br>Applies a 9-cell convolution matrix where center pixel has the weight arg1 and others weight of 1.0. The result is normalized by dividing the sum with arg1 + 8.0 (sum of the matrix).<br>any float is accepted, large value (in practice: 2048 or more) = no change<br><br>ImageFilter seem to return false if the argument(s) are out of range for the chosen filter.  

---

I needed an especially strong blur effect today and had a hard time achieving adequate results with the built-in IMG_FILTER_GAUSSIAN_BLUR filter. In order to achieve the strength of the blur I required I had to repeat the filter up to  100 times, which took way too long to be acceptable.<br><br>After a bit of searching, I found this answer to be quite a good solution to this problem: http://stackoverflow.com/a/20264482<br><br>Based on that technique, I wrote the following generic function to achieve a very strong blur in a reasonable amount of processing:<br><br>

```
<?php 

/**
 * Strong Blur
 *
 * @param resource $gdImageResource 
 * @param int $blurFactor optional 
 *  This is the strength of the blur
 *  0 = no blur, 3 = default, anything over 5 is extremely blurred
 * @return GD image resource
 * @author Martijn Frazer, idea based on http://stackoverflow.com/a/20264482
 */
function blur($gdImageResource, $blurFactor = 3)
{
  // blurFactor has to be an integer
  $blurFactor = round($blurFactor);
  
  $originalWidth = imagesx($gdImageResource);
  $originalHeight = imagesy($gdImageResource);

  $smallestWidth = ceil($originalWidth * pow(0.5, $blurFactor));
  $smallestHeight = ceil($originalHeight * pow(0.5, $blurFactor));

  // for the first run, the previous image is the original input
  $prevImage = $gdImageResource;
  $prevWidth = $originalWidth;
  $prevHeight = $originalHeight;

  // scale way down and gradually scale back up, blurring all the way
  for($i = 0; $i < $blurFactor; $i += 1)
  {    
    // determine dimensions of next image
    $nextWidth = $smallestWidth * pow(2, $i);
    $nextHeight = $smallestHeight * pow(2, $i);

    // resize previous image to next size
    $nextImage = imagecreatetruecolor($nextWidth, $nextHeight);
    imagecopyresized($nextImage, $prevImage, 0, 0, 0, 0, 
      $nextWidth, $nextHeight, $prevWidth, $prevHeight);

    // apply blur filter
    imagefilter($nextImage, IMG_FILTER_GAUSSIAN_BLUR);

    // now the new image becomes the previous image for the next step
    $prevImage = $nextImage;
    $prevWidth = $nextWidth;
      $prevHeight = $nextHeight;
  }

  // scale back to original size and blur one more time
  imagecopyresized($gdImageResource, $nextImage, 
    0, 0, 0, 0, $originalWidth, $originalHeight, $nextWidth, $nextHeight);
  imagefilter($gdImageResource, IMG_FILTER_GAUSSIAN_BLUR);

  // clean up
  imagedestroy($prevImage);

  // return result
  return $gdImageResource;
}
?>
```
  

---

Here is an alternative to IMG_FILTER_COLORIZE filter, but taking the alpha parameter of each pixel in account.<br><br>

```
<?php
function rgba_colorize($img, $color)
{
    imagesavealpha($img, true);
    imagealphablending($img, true);

    $img_x = imagesx($img);
    $img_y = imagesy($img);
    for ($x = 0; $x < $img_x; ++$x)
    {
        for ($y = 0; $y < $img_y; ++$y)
        {
            $rgba = imagecolorsforindex($img, imagecolorat($img, $x, $y));
            $color_alpha = imagecolorallocatealpha($img, $color[0], $color[1], $color[2], $rgba['alpha']);
            imagesetpixel($img, $x, $y, $color_alpha);
            imagecolordeallocate($img, $color_alpha);
        }
    }
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.imagefilter.php)

**[To root](/README.md)**