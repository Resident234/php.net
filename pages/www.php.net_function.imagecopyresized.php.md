# imagecopyresized



Most of the examples below don&apos;t keep the proportions properly. They keep using if/else for the height/width..and forgetting that you might have a max height AND a max width, not one or the other.<br><br>/**<br>* Resize an image and keep the proportions<br>* @author Allison Beckwith &lt;allison@planetargon.com&gt;<br>* @param string $filename<br>* @param integer $max_width<br>* @param integer $max_height<br>* @return image<br>*/<br>function resizeImage($filename, $max_width, $max_height)<br>{<br>    list($orig_width, $orig_height) = getimagesize($filename);<br><br>    $width = $orig_width;<br>    $height = $orig_height;<br><br>    # taller<br>    if ($height &gt; $max_height) {<br>        $width = ($max_height / $height) * $width;<br>        $height = $max_height;<br>    }<br><br>    # wider<br>    if ($width &gt; $max_width) {<br>        $height = ($max_width / $width) * $height;<br>        $width = $max_width;<br>    }<br><br>    $image_p = imagecreatetruecolor($width, $height);<br><br>    $image = imagecreatefromjpeg($filename);<br><br>    imagecopyresampled($image_p, $image, 0, 0, 0, 0, <br>                                     $width, $height, $orig_width, $orig_height);<br><br>    return $image_p;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.imagecopyresized.php)

**[To root](/README.md)**