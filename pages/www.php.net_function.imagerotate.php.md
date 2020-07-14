# imagerotate



After some INet searches and personal try-and-failures I succeed to rotate PNG images with preserving alpha channel transparency (semi transparency).<br><br>

```
<?php
    $filename = &apos;YourFile.png&apos;;
    $rotang = 20; // Rotation angle
    $source = imagecreatefrompng($filename) or die(&apos;Error opening file &apos;.$filename);
    imagealphablending($source, false);
    imagesavealpha($source, true);

    $rotation = imagerotate($source, $rotang, imageColorAllocateAlpha($source, 0, 0, 0, 127));
    imagealphablending($rotation, false);
    imagesavealpha($rotation, true);

    header(&apos;Content-type: image/png&apos;);
    imagepng($rotation);
    imagedestroy($source);
    imagedestroy($rotation);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagerotate.php)

**[To root](/README.md)**