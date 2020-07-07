# Imagick::stripImage





StripImage also delete ICC image profile by default.
The resulting images seem to lose a lot of color information and look &quot;flat&quot; compared to their non-stripped versions.

Consider keeping the ICC profile (which causes richer colors) while removing all other EXIF data:

1. Extract the ICC profile
2. Strip EXIF data and image profile
3. Add the ICC profile back

The code is:


```
<?php
$profiles = $img-&gt;getImageProfiles(&quot;icc&quot;, true);

$img-&gt;stripImage();

if(!empty($profiles))
&#xA0; &#xA0; $img-&gt;profileImage(&quot;icc&quot;, $profiles[&apos;icc&apos;]);
?>
```



  

#



Please note that striping off the exif information without handling the orientation information available in the exif will lead to wrong orientation of the image

  

#

[Official documentation page](https://www.php.net/manual/en/imagick.stripimage.php)

**[To root](/README.md)**