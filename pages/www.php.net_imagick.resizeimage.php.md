# Imagick::resizeImage



Having to do alot of resizing, i needed to know the speeds of the different resize filters.<br>This was how long it took to resize a 5906x5906 JPEG image to 1181x1181.<br><br>FILTER_POINT took: 0.334532976151 seconds<br>FILTER_BOX took: 0.777871131897 seconds<br>FILTER_TRIANGLE took: 1.3695909977 seconds<br>FILTER_HERMITE took: 1.35866093636 seconds<br>FILTER_HANNING took: 4.88722896576 seconds<br>FILTER_HAMMING took: 4.88665103912 seconds<br>FILTER_BLACKMAN took: 4.89026689529 seconds<br>FILTER_GAUSSIAN took: 1.93553304672 seconds<br>FILTER_QUADRATIC took: 1.93322920799 seconds<br>FILTER_CUBIC took: 2.58396601677 seconds<br>FILTER_CATROM took: 2.58508896828 seconds<br>FILTER_MITCHELL took: 2.58368492126 seconds<br>FILTER_LANCZOS took: 3.74232912064 seconds<br>FILTER_BESSEL took: 4.03305602074 seconds<br>FILTER_SINC took: 4.90098690987 seconds <br><br>I ended up choosing CATROM as it has a very similar result to LANCZOS, but is significantly faster.  

#

blur:  &gt; 1 is blurry, &lt; 1 is sharp<br><br>To create a nice thumbnail (LANCZOS is the slowest filter):<br><br>

```
<?php

$thumb = new Imagick();
$thumb->readImage('myimage.gif');    $thumb->resizeImage(320,240,Imagick::FILTER_LANCZOS,1);
$thumb->writeImage('mythumb.gif');
$thumb->clear();
$thumb->destroy(); 

?>
```


Or, a shorter version of the same:



```
<?php

$thumb = new Imagick('myimage.gif');

$thumb->resizeImage(320,240,Imagick::FILTER_LANCZOS,1);
$thumb->writeImage('mythumb.gif');

$thumb->destroy(); 

?>
```
  

#

The changelog comment<br><br>  "2.1.0 Added optional fit parameter. This method now supports proportional scaling. Pass zero as either parameter for proportional scaling."<br><br>is poorly structured and therefore IMO misleading.  Yes for proportional scaling you pass 0 as either parameter... however this is *not* true if you use the optional fit param.  When bestfit == true you must specify a *non-zero* value for both columns and rows.  Note it WILL still scale proportionally e.g.<br><br>   Imagick::resizeImage ( 200, 200,  imagick::FILTER_LANCZOS, 1, TRUE)<br><br>will resize a 1000x750 image to 200x150<br><br>So for proportional resizing:<br>without "bestfit"<br>   Imagick::resizeImage ( 200, 0,  imagick::FILTER_LANCZOS, 1)<br><br>with "bestfit"<br>   Imagick::resizeImage ( 200, 200,  imagick::FILTER_LANCZOS, 1, TRUE)  

#

In our linux environment, using resizeImage with any filter produced extremely high CPU Utilization (in the range of 40-50%) while doing batch resizing.<br><br>We switched to scaleImage, which produces similar results to FILTER_BOX, and CPU Utilization dropped to 2-3%. <br><br>Using XHProf to profile the two batch jobs showed amazing decreases in CPU Time, so if you&apos;re doing a lot of picture resizing, it might be beneficial to use scaleImage instead of resizeImage, as it seems to be much much more efficient.  

#

[Official documentation page](https://www.php.net/manual/en/imagick.resizeimage.php)

**[To root](/README.md)**