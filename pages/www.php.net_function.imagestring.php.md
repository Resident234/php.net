# imagestring





Some fun with imagestring:

This function is a product of too much time..
It opens an image and create a new image with one letter instead of a pixel.



```
<?php
error_reporting(E_ALL);
/**
 * generates a image with chars instead of pixels
 *
 * @param string $url Filepath or url
 * @param string $chars The chars which should replace the pixels
 * @param int $shrpns Sharpness (2 = every second pixel, 1 = every pixel ... )
 * @param int $size 
 * @param int $weight font-weight/size
 * @return sesource
 * @author Nicolas &apos;KeksNicoh&apos; Heimann &lt;www.salamipla.net&gt;
 * @date 02nov08
 */
function pixelfuck($url, $chars=&apos;ewk34543&#xA7;G&#xA7;$&#xA7;$Tg34g4g&apos;, $shrpns=1, $size=4,$weight=2)
{
&#xA0; &#xA0; list($w, $h, $type) = getimagesize($url);
&#xA0; &#xA0; $resource = imagecreatefromstring(file_get_contents($url));
&#xA0; &#xA0; $img = imagecreatetruecolor($w*$size,$h*$size);

&#xA0; &#xA0; $cc = strlen($chars);
&#xA0; &#xA0; for($y=0;$y &lt;$h;$y+=$shrpns) 
&#xA0; &#xA0; &#xA0; &#xA0; for($x=0;$x &lt;$w;$x+=$shrpns)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; imagestring($img,$weight,$x*$size,$y*$size, $chars{@++$p%$cc}, imagecolorat($resource, $x, $y));
&#xA0; &#xA0; return $img;
}

$url = &apos;http://upload.wikimedia.org/wikipedia/commons/b/be/Manga_Icon.png&apos;;
$text = &apos;I-dont-like-manga-...-Why-do-they-have-such-big-eyes? Strange-...-WHAT-WANT-YOU-DO?&apos;;

Header(&apos;Content-Type: image/png&apos;);
imagepng(pixelfuck($url, $text, 1, 6));
?>
```


Have fun&#xA0; :)

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagestring.php)

**[To root](/README.md)**