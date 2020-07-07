# imagecreatefromstring





While downloading images from internet, it&apos;s easiest to let php decide what is the file type. So, forget using imagecreatefromjpg, imagecreatefromgif and imagecreatefrompng. Instead, this is the way to go:



```
<?php
$src = &quot;http://www.varuste.net/tiedostot/l_ylabanneri.jpg&quot;;
$image = imagecreatefromstring(file_get_contents($src));
?>
```



  

#



My site allows anonymous uploads to a web-accessible location (that will execute a script if it finds one).

Naturally, I need to verify that only harmless content is accepted. I am expecting only images, so I use:



```
<?php
&#xA0; $im = $imagecreatefromstring($USERFILE);
&#xA0; $valid = ($im != FALSE);
&#xA0; imagedestroy($im);
&#xA0; return $valid;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromstring.php)

**[To root](/README.md)**