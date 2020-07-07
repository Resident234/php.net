# Imagick::setCompressionQuality





A note for people who just couldn&apos;t get this working..



With PHP 5.1.6, the below works:





```
<?php

$img-&gt;setCompression(Imagick::COMPRESSION_JPEG);

$img-&gt;setCompressionQuality(80);

?>
```




However, with higher versions of PHP (I tried on PHP 5.2.10), the code has no effect (and there are no exceptions or warnings thrown by Imagick as well).



The code that works instead is:





```
<?php

$img-&gt;setImageCompression(Imagick::COMPRESSION_JPEG);

$img-&gt;setImageCompressionQuality(80);

?>
```




and this is backwards compatible (Works on PHP 5.1.6 as well as 5.2.10)

  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setcompressionquality.php)

**[To root](/README.md)**