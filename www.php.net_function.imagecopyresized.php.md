# imagecopyresized




<div class="phpcode"><span class="html">
Most of the examples below don&apos;t keep the proportions properly. They keep using if/else for the height/width..and forgetting that you might have a max height AND a max width, not one or the other.<br><br>/**<br>* Resize an image and keep the proportions<br>* @author Allison Beckwith &lt;allison@planetargon.com&gt;<br>* @param string $filename<br>* @param integer $max_width<br>* @param integer $max_height<br>* @return image<br>*/<br>function resizeImage($filename, $max_width, $max_height)<br>{<br>&#xA0; &#xA0; list($orig_width, $orig_height) = getimagesize($filename);<br><br>&#xA0; &#xA0; $width = $orig_width;<br>&#xA0; &#xA0; $height = $orig_height;<br><br>&#xA0; &#xA0; # taller<br>&#xA0; &#xA0; if ($height &gt; $max_height) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $width = ($max_height / $height) * $width;<br>&#xA0; &#xA0; &#xA0; &#xA0; $height = $max_height;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; # wider<br>&#xA0; &#xA0; if ($width &gt; $max_width) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $height = ($max_width / $width) * $height;<br>&#xA0; &#xA0; &#xA0; &#xA0; $width = $max_width;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; $image_p = imagecreatetruecolor($width, $height);<br><br>&#xA0; &#xA0; $image = imagecreatefromjpeg($filename);<br><br>&#xA0; &#xA0; imagecopyresampled($image_p, $image, 0, 0, 0, 0, <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $width, $height, $orig_width, $orig_height);<br><br>&#xA0; &#xA0; return $image_p;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopyresized.php)

**[⬆ to root](/)**