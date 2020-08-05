# imagesavealpha



After much trial and error and gnashing of teeth I finally figured out how to composite a png with an 8-bit alpha onto a jpg. This was not obvious to me so I thought I&apos;d share. Hope it helps.<br><br>I&apos;m using this to create a framed thumbnail image:<br><br>

```
<?php
// load the frame image (png with 8-bit transparency)
$frame = imagecreatefrompng('path/to/frame.png');

// load the thumbnail image
$thumb = imagecreatefromjpeg('path/to/thumbnail.jpg');

// get the dimensions of the frame, which we'll also be using for the
// composited final image.
$width = imagesx( $frame );
$height = imagesy( $frame );

// create the destination/output image.
$img=imagecreatetruecolor( $width, $height );

// enable alpha blending on the destination image.
imagealphablending($img, true);

// Allocate a transparent color and fill the new image with it.
// Without this the image will have a black background instead of being transparent.
$transparent = imagecolorallocatealpha( $img, 0, 0, 0, 127 );
imagefill( $img, 0, 0, $transparent );

// copy the thumbnail into the output image.
imagecopyresampled($img,$thumb,32,30,0,0, 130, 100, imagesx( $thumb ), imagesy( $thumb ) );

// copy the frame into the output image (layered on top of the thumbnail)
imagecopyresampled($img,$frame,0,0,0,0, $width,$height,$width,$height);

imagealphablending($img, false);

// save the alpha
imagesavealpha($img,true);

// emit the image
header('Content-type: image/png');
imagepng( $img );

// dispose
imagedestroy($img);

// done.
exit;
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.imagesavealpha.php)

**[To root](/README.md)**