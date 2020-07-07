# imagecopymerge





I&apos;ve just checked PHP&apos;s issue tracker and a core developer says that this function was never meant to support alpha channel! and they refused to commit the provided patch! so ridicules 

Anyway i tried rodrigo&apos;s workaround and it worked quite well, thanks rodrigo for sharing it.

I came up with another idea which is much faster than his solution, (it may need a little bit more memory)



Hope it helps someone.





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

&#xA0; &#xA0; function imagecopymerge_alpha($dst_im, $src_im, $dst_x, $dst_y, $src_x, $src_y, $src_w, $src_h, $pct){

&#xA0; &#xA0; &#xA0; &#xA0; // creating a cut resource

&#xA0; &#xA0; &#xA0; &#xA0; $cut = imagecreatetruecolor($src_w, $src_h);



&#xA0; &#xA0; &#xA0; &#xA0; // copying relevant section from background to the cut resource

&#xA0; &#xA0; &#xA0; &#xA0; imagecopy($cut, $dst_im, 0, 0, $dst_x, $dst_y, $src_w, $src_h);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; // copying relevant section from watermark to the cut resource

&#xA0; &#xA0; &#xA0; &#xA0; imagecopy($cut, $src_im, 0, 0, $src_x, $src_y, $src_w, $src_h);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; // insert cut resource to destination image

&#xA0; &#xA0; &#xA0; &#xA0; imagecopymerge($dst_im, $cut, $dst_x, $dst_y, 0, 0, $src_w, $src_h, $pct);

&#xA0; &#xA0; } 



?>
```



  

#



imagecopymerge PHP function helped me to create a fast method that replaces one color by another in true color images.

Beforehand for this purpose I had to loop over all the pixels of an image and replace color pixel by pixel using imagecolorat and imagesetpixel; this method is really slow for large images.

So here is the fast one:



```
<?php

function replaceColorInImage ($image, $old_r, $old_g, $old_b, $new_r, $new_g, $new_b)
{
&#xA0; &#xA0; imagecolortransparent ($image, imagecolorallocate ($image, $old_r, $old_g, $old_b));
&#xA0; &#xA0; 
&#xA0; &#xA0; $w = imagesx ($image);
&#xA0; &#xA0; $h = imagesy ($image);
&#xA0; &#xA0; 
&#xA0; &#xA0; $resImage = imagecreatetruecolor ($w, $h);
&#xA0; &#xA0; 
&#xA0; &#xA0; imagefill ($resImage, 0, 0, imagecolorallocate ($resImage, $new_r, $new_g, $new_b));
&#xA0; &#xA0; 
&#xA0; &#xA0; imagecopymerge ($resImage, $image, 0, 0, 0, 0, $w, $h, 100);
&#xA0; &#xA0; 
&#xA0; &#xA0; return $resImage;
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopymerge.php)

**[To root](/README.md)**