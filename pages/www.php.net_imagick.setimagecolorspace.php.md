# Imagick::setImageColorspace



When converting from CMYK to RGB using this function, the image can become inverted. To fix this, use a workaround (don&apos;t forget to download the .icc files online): <br><br>

```
<?php 
// don't use this (it inverts the image) 
//    $img->setImageColorspace (imagick::COLORSPACE_RGB); 

if ($img->getImageColorspace() == Imagick::COLORSPACE_CMYK) { 
   $profiles = $img->getImageProfiles('*', false); 
   // we're only interested if ICC profile(s) exist 
   $has_icc_profile = (array_search('icc', $profiles) !== false); 
   // if it doesnt have a CMYK ICC profile, we add one 
   if ($has_icc_profile === false) { 
       $icc_cmyk = file_get_contents(dirname(__FILE__).'/USWebUncoated.icc'); 
       $img->profileImage('icc', $icc_cmyk); 
       unset($icc_cmyk); 
   } 
   // then we add an RGB profile 
   $icc_rgb = file_get_contents(dirname(__FILE__).'/sRGB_v4_ICC_preference.icc'); 
   $img->profileImage('icc', $icc_rgb); 
   unset($icc_rgb); 
} 

$img->stripImage (); // this will drop down the size of the image dramatically (removes all profiles) 
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/imagick.setimagecolorspace.php)

**[To root](/README.md)**