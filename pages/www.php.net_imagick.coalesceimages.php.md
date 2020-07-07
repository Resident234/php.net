# Imagick::coalesceImages





resize and/or crop an animated GIF





```
<?php

$image = new Imagick($file_src);



$image = $image-&gt;coalesceImages();



foreach ($image as $frame) {

&#xA0; $frame-&gt;cropImage($crop_w, $crop_h, $crop_x, $crop_y);

&#xA0; $frame-&gt;thumbnailImage($size_w, $size_h);

&#xA0; $frame-&gt;setImagePage($size_w, $size_h, 0, 0);

}



$image = $image-&gt;deconstructImages();

$image-&gt;writeImages($file_dst, true);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/imagick.coalesceimages.php)

**[To root](/README.md)**