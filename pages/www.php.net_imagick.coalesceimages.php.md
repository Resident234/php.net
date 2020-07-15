# Imagick::coalesceImages



resize and/or crop an animated GIF<br><br>

```
<?php
$image = new Imagick($file_src);

$image = $image->coalesceImages();

foreach ($image as $frame) {
  $frame->cropImage($crop_w, $crop_h, $crop_x, $crop_y);
  $frame->thumbnailImage($size_w, $size_h);
  $frame->setImagePage($size_w, $size_h, 0, 0);
}

$image = $image->deconstructImages();
$image->writeImages($file_dst, true);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.coalesceimages.php)

**[To root](/README.md)**