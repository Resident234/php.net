# Imagick::readImage





Use this to convert all pages of a PDF to JPG:





```
<?php

$imagick = new Imagick();

$imagick-&gt;readImage(&apos;myfile.pdf&apos;);

$imagick-&gt;writeImages(&apos;converted.jpg&apos;, false);

?>
```




If you need better quality, try adding $imagick-&gt;setResolution(150, 150); before reading the file!



If you experience transparency problems when converting PDF to JPEG (black background), try flattening your file:





```
<?php

$imagick = new Imagick();

$imagick-&gt;readImage(&apos;myfile.pdf[0]&apos;);

$imagick = $imagick-&gt;flattenImages();

$imagick-&gt;writeFile(&apos;pageone.jpg&apos;);

?>
```




In order to read pages from a PDF-file use [PAGENUMBER] after the filename (pages start from zero!).



Example: Read page #1 from test.pdf





```
<?php

$imagick = new Imagick();

$imagick-&gt;readImage(&apos;test.pdf[0]&apos;);

$imagick-&gt;writeImage(&apos;page_one.jpg&apos;);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/imagick.readimage.php)

**[To root](/README.md)**