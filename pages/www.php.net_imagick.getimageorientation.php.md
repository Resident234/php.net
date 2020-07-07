# Imagick::getImageOrientation





Here&apos;s how you can use the getImageOrientation() information to auto-rotate images to their correct orientation...





```
<?php

// Note: $image is an Imagick object, not a filename! See example use below.

function autoRotateImage($image) {

&#xA0; &#xA0; $orientation = $image-&gt;getImageOrientation();



&#xA0; &#xA0; switch($orientation) {

&#xA0; &#xA0; &#xA0; &#xA0; case imagick::ORIENTATION_BOTTOMRIGHT: 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image-&gt;rotateimage(&quot;#000&quot;, 180); // rotate 180 degrees

&#xA0; &#xA0; &#xA0; &#xA0; break;



&#xA0; &#xA0; &#xA0; &#xA0; case imagick::ORIENTATION_RIGHTTOP:

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image-&gt;rotateimage(&quot;#000&quot;, 90); // rotate 90 degrees CW

&#xA0; &#xA0; &#xA0; &#xA0; break;



&#xA0; &#xA0; &#xA0; &#xA0; case imagick::ORIENTATION_LEFTBOTTOM: 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image-&gt;rotateimage(&quot;#000&quot;, -90); // rotate 90 degrees CCW

&#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; }



&#xA0; &#xA0; // Now that it&apos;s auto-rotated, make sure the EXIF data is correct in case the EXIF gets saved with the image!

&#xA0; &#xA0; $image-&gt;setImageOrientation(imagick::ORIENTATION_TOPLEFT);

}

?>
```




Example use:





```
<?php

$image = new Imagick(&apos;my-image-file.jpg&apos;);

autoRotateImage($image);

// - Do other stuff to the image here -

$image-&gt;writeImage(&apos;result-image.jpg&apos;);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/imagick.getimageorientation.php)

**[To root](/README.md)**