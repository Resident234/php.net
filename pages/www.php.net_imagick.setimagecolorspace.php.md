# Imagick::setImageColorspace





When converting from CMYK to RGB using this function, the image can become inverted. To fix this, use a workaround (don&apos;t forget to download the .icc files online): 





```
<?php 

// don&apos;t use this (it inverts the image) 

//&#xA0; &#xA0; $img-&gt;setImageColorspace (imagick::COLORSPACE_RGB); 



if ($img-&gt;getImageColorspace() == Imagick::COLORSPACE_CMYK) { 

&#xA0;&#xA0; $profiles = $img-&gt;getImageProfiles(&apos;*&apos;, false); 

&#xA0;&#xA0; // we&apos;re only interested if ICC profile(s) exist 

&#xA0;&#xA0; $has_icc_profile = (array_search(&apos;icc&apos;, $profiles) !== false); 

&#xA0;&#xA0; // if it doesnt have a CMYK ICC profile, we add one 

&#xA0;&#xA0; if ($has_icc_profile === false) { 

&#xA0; &#xA0; &#xA0;&#xA0; $icc_cmyk = file_get_contents(dirname(__FILE__).&apos;/USWebUncoated.icc&apos;); 

&#xA0; &#xA0; &#xA0;&#xA0; $img-&gt;profileImage(&apos;icc&apos;, $icc_cmyk); 

&#xA0; &#xA0; &#xA0;&#xA0; unset($icc_cmyk); 

&#xA0;&#xA0; } 

&#xA0;&#xA0; // then we add an RGB profile 

&#xA0;&#xA0; $icc_rgb = file_get_contents(dirname(__FILE__).&apos;/sRGB_v4_ICC_preference.icc&apos;); 

&#xA0;&#xA0; $img-&gt;profileImage(&apos;icc&apos;, $icc_rgb); 

&#xA0;&#xA0; unset($icc_rgb); 

} 



$img-&gt;stripImage (); // this will drop down the size of the image dramatically (removes all profiles) 

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setimagecolorspace.php)

**[To root](/README.md)**