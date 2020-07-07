# imagecolortransparent





I&apos;ve made a very simple script that will retain transparency of images especially when resizing.



NOTE: Transparency is only supported on GIF and PNG files.



Parameters:



$new_image = image resource identifier such as returned by imagecreatetruecolor(). must be passed by reference

$image_source = image resource identifier returned by imagecreatefromjpeg, imagecreatefromgif and imagecreatefrompng. must be passed by reference





```
<?php

function setTransparency($new_image,$image_source)

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $transparencyIndex = imagecolortransparent($image_source);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $transparencyColor = array(&apos;red&apos; =&gt; 255, &apos;green&apos; =&gt; 255, &apos;blue&apos; =&gt; 255);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($transparencyIndex &gt;= 0) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $transparencyColor&#xA0; &#xA0; = imagecolorsforindex($image_source, $transparencyIndex);&#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $transparencyIndex&#xA0; &#xA0; = imagecolorallocate($new_image, $transparencyColor[&apos;red&apos;], $transparencyColor[&apos;green&apos;], $transparencyColor[&apos;blue&apos;]);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; imagefill($new_image, 0, 0, $transparencyIndex);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; imagecolortransparent($new_image, $transparencyIndex);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; } 

?>
```






Sample Usage: (resizing)





```
<?php

$image_source = imagecreatefrompng(&apos;test.png&apos;);

$new_image = imagecreatetruecolor($width, $height);

setTransparency($new_image,$image_source);

imagecopyresampled($new_image, $image_source, 0, 0, 0, 0, $new_width, $new_height, $old_width, $old_height);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecolortransparent.php)

**[To root](/README.md)**