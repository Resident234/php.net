# image_type_to_mime_type



If you are working with Images only and you need mime type (e.g. for headers), then this is a fast and reliable technique:<br> <br>

```
<?php
$file = 'path/to/image.jpg';
$image_mime = image_type_to_mime_type(exif_imagetype($file));
?>
```
<br><br>It will output true image mime type even if you rename your image file.  

#

[Official documentation page](https://www.php.net/manual/en/function.image-type-to-mime-type.php)

**[To root](/README.md)**