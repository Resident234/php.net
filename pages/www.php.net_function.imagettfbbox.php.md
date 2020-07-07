# imagettfbbox





I wrote a simple function that calculates the *exact* bounding box (single pixel precision).

The function returns an associative array with these keys:

left, top:&#xA0; coordinates you will pass to imagettftext 

width, height: dimension of the image you have to create





```
<?php

function calculateTextBox($font_size, $font_angle, $font_file, $text) {

&#xA0; $box&#xA0;&#xA0; = imagettfbbox($font_size, $font_angle, $font_file, $text);

&#xA0; if( !$box )

&#xA0; &#xA0; return false;

&#xA0; $min_x = min( array($box[0], $box[2], $box[4], $box[6]) );

&#xA0; $max_x = max( array($box[0], $box[2], $box[4], $box[6]) );

&#xA0; $min_y = min( array($box[1], $box[3], $box[5], $box[7]) );

&#xA0; $max_y = max( array($box[1], $box[3], $box[5], $box[7]) );

&#xA0; $width&#xA0; = ( $max_x - $min_x );

&#xA0; $height = ( $max_y - $min_y );

&#xA0; $left&#xA0;&#xA0; = abs( $min_x ) + $width;

&#xA0; $top&#xA0; &#xA0; = abs( $min_y ) + $height;

&#xA0; // to calculate the exact bounding box i write the text in a large image

&#xA0; $img&#xA0; &#xA0;&#xA0; = @imagecreatetruecolor( $width &lt;&lt; 2, $height &lt;&lt; 2 );

&#xA0; $white&#xA0;&#xA0; =&#xA0; imagecolorallocate( $img, 255, 255, 255 );

&#xA0; $black&#xA0;&#xA0; =&#xA0; imagecolorallocate( $img, 0, 0, 0 );

&#xA0; imagefilledrectangle($img, 0, 0, imagesx($img), imagesy($img), $black);

&#xA0; // for sure the text is completely in the image!

&#xA0; imagettftext( $img, $font_size,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $font_angle, $left, $top,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $white, $font_file, $text);

&#xA0; // start scanning (0=&gt; black =&gt; empty)

&#xA0; $rleft&#xA0; = $w4 = $width&lt;&lt;2;

&#xA0; $rright = 0;

&#xA0; $rbottom&#xA0;&#xA0; = 0;

&#xA0; $rtop = $h4 = $height&lt;&lt;2;

&#xA0; for( $x = 0; $x &lt; $w4; $x++ )

&#xA0; &#xA0; for( $y = 0; $y &lt; $h4; $y++ )

&#xA0; &#xA0; &#xA0; if( imagecolorat( $img, $x, $y ) ){

&#xA0; &#xA0; &#xA0; &#xA0; $rleft&#xA0;&#xA0; = min( $rleft, $x );

&#xA0; &#xA0; &#xA0; &#xA0; $rright&#xA0; = max( $rright, $x );

&#xA0; &#xA0; &#xA0; &#xA0; $rtop&#xA0; &#xA0; = min( $rtop, $y );

&#xA0; &#xA0; &#xA0; &#xA0; $rbottom = max( $rbottom, $y );

&#xA0; &#xA0; &#xA0; }

&#xA0; // destroy img and serve the result

&#xA0; imagedestroy( $img );

&#xA0; return array( &quot;left&quot;&#xA0;&#xA0; =&gt; $left - $rleft,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;top&quot;&#xA0; &#xA0; =&gt; $top&#xA0; - $rtop,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;width&quot;&#xA0; =&gt; $rright - $rleft + 1,

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;height&quot; =&gt; $rbottom - $rtop + 1 );

}

?>
```



  

#



Please note that as imageTTFBbox and imageTTFText functions return an array of coordinates which could be negative numbers care must be taken with height and width calculations.

The rigth way to do that is to use the abs() function:

for an horizontal text:

$box = @imageTTFBbox($size,0,$font,$text);
$width = abs($box[4] - $box[0]);
$height = abs($box[5] - $box[1]);

Then to center your text at ($x,$y) position the code should be like that:

$x -= $width/2;
$y += $heigth/2;

imageTTFText($img,$size,0,$x,$y,$color,$font,$text);

this because (0,0) page origin is topleft page corner and (0,0) text origin is lower-left readable text corner.

Hope this help.

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagettfbbox.php)

**[To root](/README.md)**