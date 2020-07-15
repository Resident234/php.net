# imagecreatetruecolor



HOW THE CHECK THE MEMORY BEFORE CREATING AN IMAGE?<br><br>I worked on a script where I delt with large images. Very often the error "out of memory" occured. So I had to figure out, how to check the memory BEFORE creating an image. I didn&apos;t find a solution on the web, so I ran my own test script. It built lots of images to compute the needed memory. I found out, that the formula mem = x * y * channel was not enough. There was an additional multiplier that varied. As a result 1.7 worked best. So the formula is: mem = x * y * channel * 1.7.<br><br>I post this script here hoping it is useful to someone who will run into the same problems.<br><br>

```
<?php
//-------------------------------------------------- DEFINE MAXMEM
define ("MAXMEM", 32*1024*1024);  //--- memory limit (32M) ---

//-------------------------------------------------- ENOUGH MEMORY ?
function enoughmem ($x, $y, $rgb=3) {
    return ( $x * $y * $rgb * 1.7 &lt; MAXMEM - memory_get_usage() );
}

//-------------------------------------------------- SIMPLE EXAMPLE
list ($x, $y) = @getimagesize ('your_img.jpg');  //--- get size of img ---
if (enoughmem($x,$y)) {
    $img = @imagecreatefromjpeg ('your_img.jpg');  //--- open img file ---
    $thumb = 200;  //--- max. size of thumb ---
    if ($x &gt; $y) {
        $tx = $thumb;  //--- landscape ---
        $ty = round($thumb / $x * $y);
    } else {
        $tx = round($thumb / $y * $x);  //--- portrait ---
        $ty = $thumb;
    }
    if (enoughmem($tx,$ty)) {
        $thb = imagecreatetruecolor ($tx, $ty);  //--- create thumbnail ---
        imagecopyresampled ($thb,$img, 0,0, 0,0, $tx,$ty, $x,$y);
        imagejpeg ($thb, 'your_thumbnail.jpg', 80);
        imagedestroy ($thb);
    }
    imagedestroy ($img);
}

//--------------------------------------------------
//--- to check the memory working with           ---
//--- b/w-image or gif use:                      ---
//--------------------------------------------------
$check = enoughmem ($x, $y, 1);
?>
```
<br><br>Taking the opportunity, I thank everyone here at php.net for the great manual and the useful user scripts.  

#

If you want to create a *transparent* PNG image, where the background is fully transparent, and all draw operations happen on-top of this, then do the following:<br><br>

```
<?php
    $png = imagecreatetruecolor(800, 600);
    imagesavealpha($png, true);

    $trans_colour = imagecolorallocatealpha($png, 0, 0, 0, 127);
    imagefill($png, 0, 0, $trans_colour);
    
    $red = imagecolorallocate($png, 255, 0, 0);
    imagefilledellipse($png, 400, 300, 400, 300, $red);
    
    header("Content-type: image/png");
    imagepng($png);
?>
```
<br><br>What you do is create a true colour image, make sure that the alpha save-state is on, then fill the image with a colour that has had its alpha level set to fully transparent (127).<br><br>The resulting PNG from the code above will have a red circle on a fully transparent background (drag the image into Photoshop to see for yourself)  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatetruecolor.php)

**[To root](/README.md)**