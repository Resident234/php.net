# imagecreatefrompng



If you&apos;re trying to load a translucent png-24 image but are finding an absence of transparency (like it&apos;s black), you need to enable alpha channel AND save the setting. I&apos;m new to GD and it took me almost two hours to figure this out.<br><br>

```
<?php
$imgPng = imageCreateFromPng($strImagePath);
imageAlphaBlending($imgPng, true);
imageSaveAlpha($imgPng, true);

/* Output image to browser */
header("Content-type: image/png");
imagePng($imgPng); 
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefrompng.php)

**[To root](/README.md)**