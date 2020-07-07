# imagecopyresized





Most of the examples below don&apos;t keep the proportions properly. They keep using if/else for the height/width..and forgetting that you might have a max height AND a max width, not one or the other.

/**
* Resize an image and keep the proportions
* @author Allison Beckwith &lt;allison@planetargon.com&gt;
* @param string $filename
* @param integer $max_width
* @param integer $max_height
* @return image
*/
function resizeImage($filename, $max_width, $max_height)
{
&#xA0; &#xA0; list($orig_width, $orig_height) = getimagesize($filename);

&#xA0; &#xA0; $width = $orig_width;
&#xA0; &#xA0; $height = $orig_height;

&#xA0; &#xA0; # taller
&#xA0; &#xA0; if ($height &gt; $max_height) {
&#xA0; &#xA0; &#xA0; &#xA0; $width = ($max_height / $height) * $width;
&#xA0; &#xA0; &#xA0; &#xA0; $height = $max_height;
&#xA0; &#xA0; }

&#xA0; &#xA0; # wider
&#xA0; &#xA0; if ($width &gt; $max_width) {
&#xA0; &#xA0; &#xA0; &#xA0; $height = ($max_width / $width) * $height;
&#xA0; &#xA0; &#xA0; &#xA0; $width = $max_width;
&#xA0; &#xA0; }

&#xA0; &#xA0; $image_p = imagecreatetruecolor($width, $height);

&#xA0; &#xA0; $image = imagecreatefromjpeg($filename);

&#xA0; &#xA0; imagecopyresampled($image_p, $image, 0, 0, 0, 0, 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $width, $height, $orig_width, $orig_height);

&#xA0; &#xA0; return $image_p;
}

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopyresized.php)

**[To root](/README.md)**