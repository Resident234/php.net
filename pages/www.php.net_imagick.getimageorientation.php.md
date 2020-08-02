# Imagick::getImageOrientation



Here&apos;s how you can use the getImageOrientation() information to auto-rotate images to their correct orientation...<br><br>

```
<?php
// Note: $image is an Imagick object, not a filename! See example use below.
function autoRotateImage($image) {
    $orientation = $image->getImageOrientation();

    switch($orientation) {
        case imagick::ORIENTATION_BOTTOMRIGHT: 
            $image->rotateimage("#000", 180); // rotate 180 degrees
        break;

        case imagick::ORIENTATION_RIGHTTOP:
            $image->rotateimage("#000", 90); // rotate 90 degrees CW
        break;

        case imagick::ORIENTATION_LEFTBOTTOM: 
            $image->rotateimage("#000", -90); // rotate 90 degrees CCW
        break;
    }

    // Now that it's auto-rotated, make sure the EXIF data is correct in case the EXIF gets saved with the image!
    $image->setImageOrientation(imagick::ORIENTATION_TOPLEFT);
}
?>
```


Example use:



```
<?php
$image = new Imagick('my-image-file.jpg');
autoRotateImage($image);
// - Do other stuff to the image here -
$image->writeImage('result-image.jpg');
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/imagick.getimageorientation.php)

**[To root](/README.md)**