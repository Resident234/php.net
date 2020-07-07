# imagecreatetruecolor





HOW THE CHECK THE MEMORY BEFORE CREATING AN IMAGE?

I worked on a script where I delt with large images. Very often the error &quot;out of memory&quot; occured. So I had to figure out, how to check the memory BEFORE creating an image. I didn&apos;t find a solution on the web, so I ran my own test script. It built lots of images to compute the needed memory. I found out, that the formula mem = x * y * channel was not enough. There was an additional multiplier that varied. As a result 1.7 worked best. So the formula is: mem = x * y * channel * 1.7.

I post this script here hoping it is useful to someone who will run into the same problems.



```
<?php
//-------------------------------------------------- DEFINE MAXMEM
define (&quot;MAXMEM&quot;, 32*1024*1024);&#xA0; //--- memory limit (32M) ---

//-------------------------------------------------- ENOUGH MEMORY ?
function enoughmem ($x, $y, $rgb=3) {
&#xA0; &#xA0; return ( $x * $y * $rgb * 1.7 &lt; MAXMEM - memory_get_usage() );
}

//-------------------------------------------------- SIMPLE EXAMPLE
list ($x, $y) = @getimagesize (&apos;your_img.jpg&apos;);&#xA0; //--- get size of img ---
if (enoughmem($x,$y)) {
&#xA0; &#xA0; $img = @imagecreatefromjpeg (&apos;your_img.jpg&apos;);&#xA0; //--- open img file ---
&#xA0; &#xA0; $thumb = 200;&#xA0; //--- max. size of thumb ---
&#xA0; &#xA0; if ($x &gt; $y) {
&#xA0; &#xA0; &#xA0; &#xA0; $tx = $thumb;&#xA0; //--- landscape ---
&#xA0; &#xA0; &#xA0; &#xA0; $ty = round($thumb / $x * $y);
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; $tx = round($thumb / $y * $x);&#xA0; //--- portrait ---
&#xA0; &#xA0; &#xA0; &#xA0; $ty = $thumb;
&#xA0; &#xA0; }
&#xA0; &#xA0; if (enoughmem($tx,$ty)) {
&#xA0; &#xA0; &#xA0; &#xA0; $thb = imagecreatetruecolor ($tx, $ty);&#xA0; //--- create thumbnail ---
&#xA0; &#xA0; &#xA0; &#xA0; imagecopyresampled ($thb,$img, 0,0, 0,0, $tx,$ty, $x,$y);
&#xA0; &#xA0; &#xA0; &#xA0; imagejpeg ($thb, &apos;your_thumbnail.jpg&apos;, 80);
&#xA0; &#xA0; &#xA0; &#xA0; imagedestroy ($thb);
&#xA0; &#xA0; }
&#xA0; &#xA0; imagedestroy ($img);
}

//--------------------------------------------------
//--- to check the memory working with&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; ---
//--- b/w-image or gif use:&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ---
//--------------------------------------------------
$check = enoughmem ($x, $y, 1);
?>
```


Taking the opportunity, I thank everyone here at php.net for the great manual and the useful user scripts.

  

#



If you want to create a *transparent* PNG image, where the background is fully transparent, and all draw operations happen on-top of this, then do the following:



```
<?php
&#xA0; &#xA0; $png = imagecreatetruecolor(800, 600);
&#xA0; &#xA0; imagesavealpha($png, true);

&#xA0; &#xA0; $trans_colour = imagecolorallocatealpha($png, 0, 0, 0, 127);
&#xA0; &#xA0; imagefill($png, 0, 0, $trans_colour);
&#xA0; &#xA0; 
&#xA0; &#xA0; $red = imagecolorallocate($png, 255, 0, 0);
&#xA0; &#xA0; imagefilledellipse($png, 400, 300, 400, 300, $red);
&#xA0; &#xA0; 
&#xA0; &#xA0; header(&quot;Content-type: image/png&quot;);
&#xA0; &#xA0; imagepng($png);
?>
```


What you do is create a true colour image, make sure that the alpha save-state is on, then fill the image with a colour that has had its alpha level set to fully transparent (127).

The resulting PNG from the code above will have a red circle on a fully transparent background (drag the image into Photoshop to see for yourself)

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatetruecolor.php)

**[To root](/README.md)**