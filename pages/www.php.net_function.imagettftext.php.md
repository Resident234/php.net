# imagettftext



If you&apos;re looking for easy text alignment, you need to use the imagettfbbox() command. When given the correct parameters, it will return the boundaries of your to-be-made text field in an array, which will allow you to calculate the x and y coordinate that you need to use for centering or aligning your text.<br><br>A horizontal centering example:<br><br>

```
<?php

$tb = imagettfbbox(17, 0, 'airlock.ttf', 'Hello world!');

?>
```


$tb would contain:

Array
(
    [0] => 0 // lower left X coordinate
    [1] => -1 // lower left Y coordinate
    [2] => 198 // lower right X coordinate
    [3] => -1 // lower right Y coordinate
    [4] => 198 // upper right X coordinate
    [5] => -20 // upper right Y coordinate
    [6] => 0 // upper left X coordinate
    [7] => -20 // upper left Y coordinate
)

For horizontal alignment, we need to substract the "text box's" width { $tb[2] or $tb[4] } from the image's width and then substract by two.

Saying you have a 200px wide image, you could do something like this:



```
<?php

$x = ceil((200 - $tb[2]) / 2); // lower left X coordinate for text
imagettftext($im, 17, 0, $x, $y, $tc, 'airlock.ttf', 'Hello world!'); // write text to image

?>
```
<br><br>This&apos;ll give you perfect horizontal center alignment for your text, give or take 1 pixel. Have fun!  

---

For your general edification: The following drop-in function will place a block of fully justified text onto a GD image. It is a little CPU heavy, so I suggest caching the output rather than doing it on-the-fly. <br><br>Arguments: <br><br>$image - the GD handle of the target canvas <br>$size - text size <br>$angle - slope of text (does not work very well), leave at 0 for horizontal text <br>$left - no. of pixels from left to start block <br>$top - no. of pixels from top to start block <br>$color - handle for colour (imagecolorallocate result) <br>$font - path to .ttf font <br>$text - the text to wrap and justify <br>$max_width - the width of the text block within which the text should be wrapped and fully justified <br>$minspacing - the minimum number of pixels between words <br>$linespacing - a multiplier of line height (1 for normal spacing; 1.5 for line-and-a-half etc.)<br><br>eg.<br>$image = ImageCreateFromJPEG( "sample.jpg" );<br>$cor = imagecolorallocate($image, 0, 0, 0);<br>$font = &apos;arial.ttf&apos;;<br>$a = imagettftextjustified($image, 20, 0, 50, 50, $color, $font, "Shree", 500, $minspacing=3,$linespacing=1);<br>header(&apos;Content-type: image/jpeg&apos;);<br>imagejpeg($image,NULL,100);<br><br>function imagettftextjustified(&amp;$image, $size, $angle, $left, $top, $color, $font, $text, $max_width, $minspacing=3,$linespacing=1)<br>{<br>$wordwidth = array();<br>$linewidth = array();<br>$linewordcount = array();<br>$largest_line_height = 0;<br>$lineno=0;<br>$words=explode(" ",$text);<br>$wln=0;<br>$linewidth[$lineno]=0;<br>$linewordcount[$lineno]=0;<br>foreach ($words as $word)<br>{<br>$dimensions = imagettfbbox($size, $angle, $font, $word);<br>$line_width = $dimensions[2] - $dimensions[0];<br>$line_height = $dimensions[1] - $dimensions[7];<br>if ($line_height&gt;$largest_line_height) $largest_line_height=$line_height;<br>if (($linewidth[$lineno]+$line_width+$minspacing)&gt;$max_width)<br>{<br>$lineno++;<br>$linewidth[$lineno]=0;<br>$linewordcount[$lineno]=0;<br>$wln=0;<br>}<br>$linewidth[$lineno]+=$line_width+$minspacing;<br>$wordwidth[$lineno][$wln]=$line_width;<br>$wordtext[$lineno][$wln]=$word;<br>$linewordcount[$lineno]++;<br>$wln++;<br>}<br>for ($ln=0;$ln&lt;=$lineno;$ln++)<br>{<br>$slack=$max_width-$linewidth[$ln];<br>if (($linewordcount[$ln]&gt;1)&amp;&amp;($ln!=$lineno)) $spacing=($slack/($linewordcount[$ln]-1));<br>else $spacing=$minspacing;<br>$x=0;<br>for ($w=0;$w&lt;$linewordcount[$ln];$w++)<br>{<br>imagettftext($image, $size, $angle, $left + intval($x), $top + $largest_line_height + ($largest_line_height * $ln * $linespacing), $color, $font, $wordtext[$ln][$w]);<br>$x+=$wordwidth[$ln][$w]+$spacing+$minspacing;<br>}<br>}<br>return true;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.imagettftext.php)

**[To root](/README.md)**