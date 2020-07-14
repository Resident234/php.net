# imagecopymerge



I&apos;ve just checked PHP&apos;s issue tracker and a core developer says that this function was never meant to support alpha channel! and they refused to commit the provided patch! so ridicules <br>Anyway i tried rodrigo&apos;s workaround and it worked quite well, thanks rodrigo for sharing it.<br>I came up with another idea which is much faster than his solution, (it may need a little bit more memory)<br><br>Hope it helps someone.<br><br>

```
<?php
/**
 * PNG ALPHA CHANNEL SUPPORT for imagecopymerge();
 * by Sina Salek
 *
 * Bugfix by Ralph Voigt (bug which causes it
 * to work only for $src_x = $src_y = 0. 
 * Also, inverting opacity is not necessary.)
 * 08-JAN-2011
 *
 **/
    function imagecopymerge_alpha($dst_im, $src_im, $dst_x, $dst_y, $src_x, $src_y, $src_w, $src_h, $pct){
        // creating a cut resource
        $cut = imagecreatetruecolor($src_w, $src_h);

        // copying relevant section from background to the cut resource
        imagecopy($cut, $dst_im, 0, 0, $dst_x, $dst_y, $src_w, $src_h);
        
        // copying relevant section from watermark to the cut resource
        imagecopy($cut, $src_im, 0, 0, $src_x, $src_y, $src_w, $src_h);
        
        // insert cut resource to destination image
        imagecopymerge($dst_im, $cut, $dst_x, $dst_y, 0, 0, $src_w, $src_h, $pct);
    } 

?>
```
  

#

imagecopymerge PHP function helped me to create a fast method that replaces one color by another in true color images.<br><br>Beforehand for this purpose I had to loop over all the pixels of an image and replace color pixel by pixel using imagecolorat and imagesetpixel; this method is really slow for large images.<br><br>So here is the fast one:<br><br>

```
<?php

function replaceColorInImage ($image, $old_r, $old_g, $old_b, $new_r, $new_g, $new_b)
{
    imagecolortransparent ($image, imagecolorallocate ($image, $old_r, $old_g, $old_b));
    
    $w = imagesx ($image);
    $h = imagesy ($image);
    
    $resImage = imagecreatetruecolor ($w, $h);
    
    imagefill ($resImage, 0, 0, imagecolorallocate ($resImage, $new_r, $new_g, $new_b));
    
    imagecopymerge ($resImage, $image, 0, 0, 0, 0, $w, $h, 100);
    
    return $resImage;
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopymerge.php)

**[To root](/README.md)**