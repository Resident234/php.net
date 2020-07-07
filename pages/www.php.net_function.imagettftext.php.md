# imagettftext





If you&apos;re looking for easy text alignment, you need to use the imagettfbbox() command. When given the correct parameters, it will return the boundaries of your to-be-made text field in an array, which will allow you to calculate the x and y coordinate that you need to use for centering or aligning your text.

A horizontal centering example:



```
<?php

$tb = imagettfbbox(17, 0, &apos;airlock.ttf&apos;, &apos;Hello world!&apos;);

?>
```


$tb would contain:

Array
(
&#xA0; &#xA0; [0] =&gt; 0 // lower left X coordinate
&#xA0; &#xA0; [1] =&gt; -1 // lower left Y coordinate
&#xA0; &#xA0; [2] =&gt; 198 // lower right X coordinate
&#xA0; &#xA0; [3] =&gt; -1 // lower right Y coordinate
&#xA0; &#xA0; [4] =&gt; 198 // upper right X coordinate
&#xA0; &#xA0; [5] =&gt; -20 // upper right Y coordinate
&#xA0; &#xA0; [6] =&gt; 0 // upper left X coordinate
&#xA0; &#xA0; [7] =&gt; -20 // upper left Y coordinate
)

For horizontal alignment, we need to substract the &quot;text box&apos;s&quot; width { $tb[2] or $tb[4] } from the image&apos;s width and then substract by two.

Saying you have a 200px wide image, you could do something like this:



```
<?php

$x = ceil((200 - $tb[2]) / 2); // lower left X coordinate for text
imagettftext($im, 17, 0, $x, $y, $tc, &apos;airlock.ttf&apos;, &apos;Hello world!&apos;); // write text to image

?>
```


This&apos;ll give you perfect horizontal center alignment for your text, give or take 1 pixel. Have fun!

  

#



For your general edification: The following drop-in function will place a block of fully justified text onto a GD image. It is a little CPU heavy, so I suggest caching the output rather than doing it on-the-fly. 

Arguments: 

$image - the GD handle of the target canvas 
$size - text size 
$angle - slope of text (does not work very well), leave at 0 for horizontal text 
$left - no. of pixels from left to start block 
$top - no. of pixels from top to start block 
$color - handle for colour (imagecolorallocate result) 
$font - path to .ttf font 
$text - the text to wrap and justify 
$max_width - the width of the text block within which the text should be wrapped and fully justified 
$minspacing - the minimum number of pixels between words 
$linespacing - a multiplier of line height (1 for normal spacing; 1.5 for line-and-a-half etc.)

eg.
$image = ImageCreateFromJPEG( &quot;sample.jpg&quot; );
$cor = imagecolorallocate($image, 0, 0, 0);
$font = &apos;arial.ttf&apos;;
$a = imagettftextjustified($image, 20, 0, 50, 50, $color, $font, &quot;Shree&quot;, 500, $minspacing=3,$linespacing=1);
header(&apos;Content-type: image/jpeg&apos;);
imagejpeg($image,NULL,100);

function imagettftextjustified(&amp;$image, $size, $angle, $left, $top, $color, $font, $text, $max_width, $minspacing=3,$linespacing=1)
{
$wordwidth = array();
$linewidth = array();
$linewordcount = array();
$largest_line_height = 0;
$lineno=0;
$words=explode(&quot; &quot;,$text);
$wln=0;
$linewidth[$lineno]=0;
$linewordcount[$lineno]=0;
foreach ($words as $word)
{
$dimensions = imagettfbbox($size, $angle, $font, $word);
$line_width = $dimensions[2] - $dimensions[0];
$line_height = $dimensions[1] - $dimensions[7];
if ($line_height&gt;$largest_line_height) $largest_line_height=$line_height;
if (($linewidth[$lineno]+$line_width+$minspacing)&gt;$max_width)
{
$lineno++;
$linewidth[$lineno]=0;
$linewordcount[$lineno]=0;
$wln=0;
}
$linewidth[$lineno]+=$line_width+$minspacing;
$wordwidth[$lineno][$wln]=$line_width;
$wordtext[$lineno][$wln]=$word;
$linewordcount[$lineno]++;
$wln++;
}
for ($ln=0;$ln&lt;=$lineno;$ln++)
{
$slack=$max_width-$linewidth[$ln];
if (($linewordcount[$ln]&gt;1)&amp;&amp;($ln!=$lineno)) $spacing=($slack/($linewordcount[$ln]-1));
else $spacing=$minspacing;
$x=0;
for ($w=0;$w&lt;$linewordcount[$ln];$w++)
{
imagettftext($image, $size, $angle, $left + intval($x), $top + $largest_line_height + ($largest_line_height * $ln * $linespacing), $color, $font, $wordtext[$ln][$w]);
$x+=$wordwidth[$ln][$w]+$spacing+$minspacing;
}
}
return true;
}

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagettftext.php)

**[To root](/README.md)**