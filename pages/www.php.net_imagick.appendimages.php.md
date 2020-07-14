# Imagick::appendImages



# How to combine a multi-page pdf file into a single long image:<br><br>

```
<?php
$im1 = new Imagick();   
$im1-&gt;readImage(&apos;multi-page-pdf.pdf&apos;);
$im1-&gt;resetIterator();
# Combine multiple images into one, stacked vertically. 
$ima = $im1-&gt;appendImages(true);
$ima-&gt;setImageFormat("png");
header("Content-Type: image/png");
echo $ima;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.appendimages.php)

**[To root](/README.md)**