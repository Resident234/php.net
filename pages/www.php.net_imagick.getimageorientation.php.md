# Imagick::getImageOrientation



Here&apos;s how you can use the getImageOrientation() information to auto-rotate images to their correct orientation...<br><br>

```
<?php
// Note: $image is an Imagick object, not a filename! See example use below.
function autoRotateImage($image) {
    $orientation = $image-&gt;getImageOrientation();

    switch($orientation) {
        case imagick::ORIENTATION_BOTTOMRIGHT: 
            $image-&gt;rotateimage("#000", 180); // rotate 180 degrees
        break;

        case imagick::ORIENTATION_RIGHTTOP:
            $image-&gt;rotateimage("#000", 90); // rotate 90 degrees CW
        break;

        case imagick::ORIENTATION_LEFTBOTTOM: 
            $image-&gt;rotateimage("#000", -90); // rotate 90 degrees CCW
        break;
    }

    // Now that it&apos;s auto-rotated, make sure the EXIF data is correct in case the EXIF gets saved with the image!
    $image-&gt;setImageOrientation(imagick::ORIENTATION_TOPLEFT);
}
?>
```


Example use:



```
<?php
$image = new Imagick(&apos;my-image-file.jpg&apos;);
autoRotateImage($image);
// - Do other stuff to the image here -
$image-&gt;writeImage(&apos;result-image.jpg&apos;);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.getimageorientation.php)

**[To root](/README.md)**