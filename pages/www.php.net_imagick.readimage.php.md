# Imagick::readImage



Use this to convert all pages of a PDF to JPG:<br><br>

```
<?php
$imagick = new Imagick();
$imagick->readImage('myfile.pdf');
$imagick->writeImages('converted.jpg', false);
?>
```


If you need better quality, try adding $imagick->setResolution(150, 150); before reading the file!

If you experience transparency problems when converting PDF to JPEG (black background), try flattening your file:



```
<?php
$imagick = new Imagick();
$imagick->readImage('myfile.pdf[0]');
$imagick = $imagick->flattenImages();
$imagick->writeFile('pageone.jpg');
?>
```


In order to read pages from a PDF-file use [PAGENUMBER] after the filename (pages start from zero!).

Example: Read page #1 from test.pdf



```
<?php
$imagick = new Imagick();
$imagick->readImage('test.pdf[0]');
$imagick->writeImage('page_one.jpg');
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/imagick.readimage.php)

**[To root](/README.md)**