# imagecreatefromstring



While downloading images from internet, it&apos;s easiest to let php decide what is the file type. So, forget using imagecreatefromjpg, imagecreatefromgif and imagecreatefrompng. Instead, this is the way to go:<br><br>

```
<?php
$src = "http://www.varuste.net/tiedostot/l_ylabanneri.jpg";
$image = imagecreatefromstring(file_get_contents($src));
?>
```
  

#

My site allows anonymous uploads to a web-accessible location (that will execute a script if it finds one).<br><br>Naturally, I need to verify that only harmless content is accepted. I am expecting only images, so I use:<br><br>

```
<?php
  $im = $imagecreatefromstring($USERFILE);
  $valid = ($im != FALSE);
  imagedestroy($im);
  return $valid;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromstring.php)

**[To root](/README.md)**