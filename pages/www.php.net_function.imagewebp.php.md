# imagewebp



To convert a PNG image to Webp, we can do this:<br><br>

```
<?php

// Image
$dir = 'img/countries/';
$name = 'brazil.png';
$newName = 'brazil.webp';

// Create and save
$img = imagecreatefrompng($dir . $name);
imagepalettetotruecolor($img);
imagealphablending($img, true);
imagesavealpha($img, true);
imagewebp($img, $dir . $newName, 100);
imagedestroy($img);

?>
```
  

---

As of today (end of january 2019), WebP is now supported across all the major browsers (Edge, Chrome, Firefox, Opera).  

---

[Official documentation page](https://www.php.net/manual/en/function.imagewebp.php)

**[To root](/README.md)**