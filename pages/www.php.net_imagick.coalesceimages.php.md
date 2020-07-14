# Imagick::coalesceImages



resize and/or crop an animated GIF<br><br>

```
<?php
$image = new Imagick($file_src);

$image = $image-&gt;coalesceImages();

foreach ($image as $frame) {
  $frame-&gt;cropImage($crop_w, $crop_h, $crop_x, $crop_y);
  $frame-&gt;thumbnailImage($size_w, $size_h);
  $frame-&gt;setImagePage($size_w, $size_h, 0, 0);
}

$image = $image-&gt;deconstructImages();
$image-&gt;writeImages($file_dst, true);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.coalesceimages.php)

**[To root](/README.md)**