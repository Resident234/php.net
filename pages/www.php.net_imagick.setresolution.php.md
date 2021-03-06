# Imagick::setResolution



This method uses the "convert -density {$x_resolution}x{$y_resolution}" parameter. However be aware, that Imagick::setResolution() is much more alike the "convert -density" option than Imagick::setImageResolution()<br><br>It&apos;s very irritating that both Imagick::setResolution() and Imagick::setImageResolution() are introduced with "Sets the image resolution."<br><br>Use Imagick::setResolution() prior to reading a raster image. This method does not affect an image. However this method tells the image to which size it has to be sized in relation to images inherent resolution! With this method you are able to affect the real pixel-size of an image after reading. E.g. your image has a size of 100x100 pixels and an inherent resolution of 72. Setting the Resolution to 144 and reading this image results in a new image size of 200x200 pixels.<br><br>

```
<?php
$im = new Imagick();
$im->setResolution(144,144);
$im->readImage("test.eps");
$im->setImageFormat("png");
header("Content-Type: image/png");
echo $im;
?>
```


Use Imagick::setImageResolution() to alter the resolution of an already read image. This method does not actually change the size of an image, but has influence to methods, which depends on a given image resolution like Imagick::resampleImage(). E.g. your image has a size of 100x100 pixels and a resolution of 72. Setting ImageResolution to 144 does nothing, unless you are resampling image afterwards in relation to the ImageResolution you set!



```
<?php
$im = new Imagick();
$im->readImage("test.eps");
$im->setImageResolution(144,144);
$im->resampleImage  (288,288,imagick::FILTER_UNDEFINED,1);
$im->setImageFormat("png");
header("Content-Type: image/png");
echo $im;
?>
```


which actually does the same like



```
<?php
$im = new Imagick();
$im->readImage("test.eps");
$im->setImageResolution(72,72);
$im->resampleImage  (144,144,imagick::FILTER_UNDEFINED,1);
$im->setImageFormat("png");
header("Content-Type: image/png");
echo $im;
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/imagick.setresolution.php)

**[To root](/README.md)**