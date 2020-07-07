# imagerotate





After some INet searches and personal try-and-failures I succeed to rotate PNG images with preserving alpha channel transparency (semi transparency).



```
<?php
&#xA0; &#xA0; $filename = &apos;YourFile.png&apos;;
&#xA0; &#xA0; $rotang = 20; // Rotation angle
&#xA0; &#xA0; $source = imagecreatefrompng($filename) or die(&apos;Error opening file &apos;.$filename);
&#xA0; &#xA0; imagealphablending($source, false);
&#xA0; &#xA0; imagesavealpha($source, true);

&#xA0; &#xA0; $rotation = imagerotate($source, $rotang, imageColorAllocateAlpha($source, 0, 0, 0, 127));
&#xA0; &#xA0; imagealphablending($rotation, false);
&#xA0; &#xA0; imagesavealpha($rotation, true);

&#xA0; &#xA0; header(&apos;Content-type: image/png&apos;);
&#xA0; &#xA0; imagepng($rotation);
&#xA0; &#xA0; imagedestroy($source);
&#xA0; &#xA0; imagedestroy($rotation);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagerotate.php)

**[To root](/README.md)**