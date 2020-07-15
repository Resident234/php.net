# imagestring



Some fun with imagestring:<br><br>This function is a product of too much time..<br>It opens an image and create a new image with one letter instead of a pixel.<br><br>

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
 * @author Nicolas 'KeksNicoh' Heimann &lt;www.salamipla.net&gt;
 * @date 02nov08
 */
function pixelfuck($url, $chars='ewk34543&#xA7;G&#xA7;

```
<?php<br>error_reporting(E_ALL);<br>/**<br> * generates a image with chars instead of pixels<br> *<br> * @param string $url Filepath or url<br> * @param string $chars The chars which should replace the pixels<br> * @param int $shrpns Sharpness (2 = every second pixel, 1 = every pixel ... )<br> * @param int $size <br> * @param int $weight font-weight/size<br> * @return sesource<br> * @author Nicolas &apos;KeksNicoh&apos; Heimann &lt;www.salamipla.net&gt;<br> * @date 02nov08<br> */<br>function pixelfuck($url, $chars=&apos;ewk34543&#xA7;G&#xA7;$&#xA7;$Tg34g4g&apos;, $shrpns=1, $size=4,$weight=2)<br>{<br>    list($w, $h, $type) = getimagesize($url);<br>    $resource = imagecreatefromstring(file_get_contents($url));<br>    $img = imagecreatetruecolor($w*$size,$h*$size);<br><br>    $cc = strlen($chars);<br>    for($y=0;$y &lt;$h;$y+=$shrpns) <br>        for($x=0;$x &lt;$w;$x+=$shrpns)<br>            imagestring($img,$weight,$x*$size,$y*$size, $chars{@++$p%$cc}, imagecolorat($resource, $x, $y));<br>    return $img;<br>}<br><br>$url = &apos;http://upload.wikimedia.org/wikipedia/commons/b/be/Manga_Icon.png&apos;;<br>$text = &apos;I-dont-like-manga-...-Why-do-they-have-such-big-eyes? Strange-...-WHAT-WANT-YOU-DO?&apos;;<br><br>Header(&apos;Content-Type: image/png&apos;);<br>imagepng(pixelfuck($url, $text, 1, 6));<br>?>
```
#xA7;$Tg34g4g', $shrpns=1, $size=4,$weight=2)
{
    list($w, $h, $type) = getimagesize($url);
    $resource = imagecreatefromstring(file_get_contents($url));
    $img = imagecreatetruecolor($w*$size,$h*$size);

    $cc = strlen($chars);
    for($y=0;$y &lt;$h;$y+=$shrpns) 
        for($x=0;$x &lt;$w;$x+=$shrpns)
            imagestring($img,$weight,$x*$size,$y*$size, $chars{@++$p%$cc}, imagecolorat($resource, $x, $y));
    return $img;
}

$url = 'http://upload.wikimedia.org/wikipedia/commons/b/be/Manga_Icon.png';
$text = 'I-dont-like-manga-...-Why-do-they-have-such-big-eyes? Strange-...-WHAT-WANT-YOU-DO?';

Header('Content-Type: image/png');
imagepng(pixelfuck($url, $text, 1, 6));
?>
```
<br><br>Have fun  :)  

#

[Official documentation page](https://www.php.net/manual/en/function.imagestring.php)

**[To root](/README.md)**