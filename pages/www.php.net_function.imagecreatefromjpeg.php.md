# imagecreatefromjpeg



This function does not honour EXIF orientation data.  Pictures that are rotated using EXIF, will show up in the original orientation after being handled by imagecreatefromjpeg().  Below is a function to create an image from JPEG while honouring EXIF orientation data.<br><br>

```
<?php
    function imagecreatefromjpegexif($filename)
    {
        $img = imagecreatefromjpeg($filename);
        $exif = exif_read_data($filename);
        if ($img &amp;&amp; $exif &amp;&amp; isset($exif['Orientation']))
        {
            $ort = $exif['Orientation'];

            if ($ort == 6 || $ort == 5)
                $img = imagerotate($img, 270, null);
            if ($ort == 3 || $ort == 4)
                $img = imagerotate($img, 180, null);
            if ($ort == 8 || $ort == 7)
                $img = imagerotate($img, 90, null);

            if ($ort == 5 || $ort == 4 || $ort == 7)
                imageflip($img, IMG_FLIP_HORIZONTAL);
        }
        return $img;
    }
?>
```
  

#

This little function allows you to create an image based on the popular image types without worrying about what it is:<br><br>

```
<?php
function imageCreateFromAny($filepath) {
    $type = exif_imagetype($filepath); // [] if you don't have exif you could use getImageSize()
    $allowedTypes = array(
        1,  // [] gif
        2,  // [] jpg
        3,  // [] png
        6   // [] bmp
    );
    if (!in_array($type, $allowedTypes)) {
        return false;
    }
    switch ($type) {
        case 1 :
            $im = imageCreateFromGif($filepath);
        break;
        case 2 :
            $im = imageCreateFromJpeg($filepath);
        break;
        case 3 :
            $im = imageCreateFromPng($filepath);
        break;
        case 6 :
            $im = imageCreateFromBmp($filepath);
        break;
    }    
    return $im;  
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromjpeg.php)

**[To root](/README.md)**