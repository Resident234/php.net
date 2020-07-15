# Imagick::setCompressionQuality



A note for people who just couldn&apos;t get this working..<br><br>With PHP 5.1.6, the below works:<br><br>

```
<?php
$img->setCompression(Imagick::COMPRESSION_JPEG);
$img->setCompressionQuality(80);
?>
```


However, with higher versions of PHP (I tried on PHP 5.2.10), the code has no effect (and there are no exceptions or warnings thrown by Imagick as well).

The code that works instead is:



```
<?php
$img->setImageCompression(Imagick::COMPRESSION_JPEG);
$img->setImageCompressionQuality(80);
?>
```
<br><br>and this is backwards compatible (Works on PHP 5.1.6 as well as 5.2.10)  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setcompressionquality.php)

**[To root](/README.md)**