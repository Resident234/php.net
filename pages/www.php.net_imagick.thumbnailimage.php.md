# Imagick::thumbnailImage



Even though thumbnailImage is meant to produce the smallest file size image possible, i found it didn&apos;t. I put together this code and bordering different compression settings, found it produced the smallest file size:<br><br>

```
<?php
// Max vert or horiz resolution
$maxsize=550;

// create new Imagick object
$image = new Imagick('input_image_filename_and_location');

// Resizes to whichever is larger, width or height
if($image->getImageHeight() <= $image->getImageWidth())
{
// Resize image using the lanczos resampling algorithm based on width
$image->resizeImage($maxsize,0,Imagick::FILTER_LANCZOS,1);
}
else
{
// Resize image using the lanczos resampling algorithm based on height
$image->resizeImage(0,$maxsize,Imagick::FILTER_LANCZOS,1);
}

// Set to use jpeg compression
$image->setImageCompression(Imagick::COMPRESSION_JPEG);
// Set compression level (1 lowest quality, 100 highest quality)
$image->setImageCompressionQuality(75);
// Strip out unneeded meta data
$image->stripImage();
// Writes resultant image to output directory
$image->writeImage('output_image_filename_and_location');
// Destroys Imagick object, freeing allocated resources in the process
$image->destroy();

?>
```
<br><br>I found setCompression to not function at all and had to use setImageCompression. The stripImage call is needed and strips out unneeded meta data. You can choose whatever filter you want, but i found lanczos to be the best for image reduction, though it is more computationally heavy.  

---

[Official documentation page](https://www.php.net/manual/en/imagick.thumbnailimage.php)

**[To root](/README.md)**