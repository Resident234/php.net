# Imagick::stripImage



StripImage also delete ICC image profile by default.<br>The resulting images seem to lose a lot of color information and look "flat" compared to their non-stripped versions.<br><br>Consider keeping the ICC profile (which causes richer colors) while removing all other EXIF data:<br><br>1. Extract the ICC profile<br>2. Strip EXIF data and image profile<br>3. Add the ICC profile back<br><br>The code is:<br>

```
<?php
$profiles = $img->getImageProfiles("icc", true);

$img->stripImage();

if(!empty($profiles))
    $img->profileImage("icc", $profiles['icc']);
?>
```
  

---

Please note that striping off the exif information without handling the orientation information available in the exif will lead to wrong orientation of the image  

---

[Official documentation page](https://www.php.net/manual/en/imagick.stripimage.php)

**[To root](/README.md)**