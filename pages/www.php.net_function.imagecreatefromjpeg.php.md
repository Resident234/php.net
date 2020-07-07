# imagecreatefromjpeg





This function does not honour EXIF orientation data.&#xA0; Pictures that are rotated using EXIF, will show up in the original orientation after being handled by imagecreatefromjpeg().&#xA0; Below is a function to create an image from JPEG while honouring EXIF orientation data.



```
<?php
&#xA0; &#xA0; function imagecreatefromjpegexif($filename)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $img = imagecreatefromjpeg($filename);
&#xA0; &#xA0; &#xA0; &#xA0; $exif = exif_read_data($filename);
&#xA0; &#xA0; &#xA0; &#xA0; if ($img &amp;&amp; $exif &amp;&amp; isset($exif[&apos;Orientation&apos;]))
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ort = $exif[&apos;Orientation&apos;];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($ort == 6 || $ort == 5)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $img = imagerotate($img, 270, null);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($ort == 3 || $ort == 4)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $img = imagerotate($img, 180, null);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($ort == 8 || $ort == 7)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $img = imagerotate($img, 90, null);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($ort == 5 || $ort == 4 || $ort == 7)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; imageflip($img, IMG_FLIP_HORIZONTAL);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $img;
&#xA0; &#xA0; }
?>
```



  

#



This little function allows you to create an image based on the popular image types without worrying about what it is:





```
<?php

function imageCreateFromAny($filepath) {

&#xA0; &#xA0; $type = exif_imagetype($filepath); // [] if you don&apos;t have exif you could use getImageSize()

&#xA0; &#xA0; $allowedTypes = array(

&#xA0; &#xA0; &#xA0; &#xA0; 1,&#xA0; // [] gif

&#xA0; &#xA0; &#xA0; &#xA0; 2,&#xA0; // [] jpg

&#xA0; &#xA0; &#xA0; &#xA0; 3,&#xA0; // [] png

&#xA0; &#xA0; &#xA0; &#xA0; 6&#xA0;&#xA0; // [] bmp

&#xA0; &#xA0; );

&#xA0; &#xA0; if (!in_array($type, $allowedTypes)) {

&#xA0; &#xA0; &#xA0; &#xA0; return false;

&#xA0; &#xA0; }

&#xA0; &#xA0; switch ($type) {

&#xA0; &#xA0; &#xA0; &#xA0; case 1 :

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $im = imageCreateFromGif($filepath);

&#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; case 2 :

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $im = imageCreateFromJpeg($filepath);

&#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; case 3 :

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $im = imageCreateFromPng($filepath);

&#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; case 6 :

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $im = imageCreateFromBmp($filepath);

&#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; }&#xA0; &#xA0; 

&#xA0; &#xA0; return $im;&#xA0; 

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromjpeg.php)

**[To root](/README.md)**