# Imagick::appendImages



# How to combine a multi-page pdf file into a single long image:<br><br>

```
<?php
$im1 = new Imagick();   
$im1->readImage('multi-page-pdf.pdf');
$im1->resetIterator();
# Combine multiple images into one, stacked vertically. 
$ima = $im1->appendImages(true);
$ima->setImageFormat("png");
header("Content-Type: image/png");
echo $ima;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.appendimages.php)

**[To root](/README.md)**