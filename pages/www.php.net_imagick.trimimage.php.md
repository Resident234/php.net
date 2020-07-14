# Imagick::trimImage



After operations that change the crop of the image, like trimImage does, IM preserves the old canvas and positioning info. If you need to do additional operations on the image based on the new size, you&apos;ll need to reset this info with setImagePage. This is the equivalent of the +repage command line argument.<br><br>

```
<?php
$im-&gt;trimImage(0);
$im-&gt;setImagePage(0, 0, 0, 0);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.trimimage.php)

**[To root](/README.md)**