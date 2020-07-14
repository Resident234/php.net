# imagettfbbox



I wrote a simple function that calculates the *exact* bounding box (single pixel precision).<br>The function returns an associative array with these keys:<br>left, top:  coordinates you will pass to imagettftext <br>width, height: dimension of the image you have to create<br><br>

```
<?php
function calculateTextBox($font_size, $font_angle, $font_file, $text) {
  $box   = imagettfbbox($font_size, $font_angle, $font_file, $text);
  if( !$box )
    return false;
  $min_x = min( array($box[0], $box[2], $box[4], $box[6]) );
  $max_x = max( array($box[0], $box[2], $box[4], $box[6]) );
  $min_y = min( array($box[1], $box[3], $box[5], $box[7]) );
  $max_y = max( array($box[1], $box[3], $box[5], $box[7]) );
  $width  = ( $max_x - $min_x );
  $height = ( $max_y - $min_y );
  $left   = abs( $min_x ) + $width;
  $top    = abs( $min_y ) + $height;
  // to calculate the exact bounding box i write the text in a large image
  $img     = @imagecreatetruecolor( $width &lt;&lt; 2, $height &lt;&lt; 2 );
  $white   =  imagecolorallocate( $img, 255, 255, 255 );
  $black   =  imagecolorallocate( $img, 0, 0, 0 );
  imagefilledrectangle($img, 0, 0, imagesx($img), imagesy($img), $black);
  // for sure the text is completely in the image!
  imagettftext( $img, $font_size,
                $font_angle, $left, $top,
                $white, $font_file, $text);
  // start scanning (0=&gt; black =&gt; empty)
  $rleft  = $w4 = $width&lt;&lt;2;
  $rright = 0;
  $rbottom   = 0;
  $rtop = $h4 = $height&lt;&lt;2;
  for( $x = 0; $x &lt; $w4; $x++ )
    for( $y = 0; $y &lt; $h4; $y++ )
      if( imagecolorat( $img, $x, $y ) ){
        $rleft   = min( $rleft, $x );
        $rright  = max( $rright, $x );
        $rtop    = min( $rtop, $y );
        $rbottom = max( $rbottom, $y );
      }
  // destroy img and serve the result
  imagedestroy( $img );
  return array( "left"   =&gt; $left - $rleft,
                "top"    =&gt; $top  - $rtop,
                "width"  =&gt; $rright - $rleft + 1,
                "height" =&gt; $rbottom - $rtop + 1 );
}
?>
```
  

#

Please note that as imageTTFBbox and imageTTFText functions return an array of coordinates which could be negative numbers care must be taken with height and width calculations.<br><br>The rigth way to do that is to use the abs() function:<br><br>for an horizontal text:<br><br>$box = @imageTTFBbox($size,0,$font,$text);<br>$width = abs($box[4] - $box[0]);<br>$height = abs($box[5] - $box[1]);<br><br>Then to center your text at ($x,$y) position the code should be like that:<br><br>$x -= $width/2;<br>$y += $heigth/2;<br><br>imageTTFText($img,$size,0,$x,$y,$color,$font,$text);<br><br>this because (0,0) page origin is topleft page corner and (0,0) text origin is lower-left readable text corner.<br><br>Hope this help.  

#

[Official documentation page](https://www.php.net/manual/en/function.imagettfbbox.php)

**[To root](/README.md)**