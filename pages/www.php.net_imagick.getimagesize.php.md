# Imagick::getImageSize



Practical use to get the dimensions of the image:<br><br>

```
<?php
$image = new Imagick($image_src);
$d = $image->getImageGeometry();
$w = $d['width'];
$h = $d['height'];
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/imagick.getimagesize.php)

**[To root](/README.md)**