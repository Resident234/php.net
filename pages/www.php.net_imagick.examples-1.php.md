# Basic usage





Be careful when loading multiple images by passing an array to a new Imagick object. This is from Example #2:



```
<?php

$images = new Imagick(glob(&apos;images/*.JPG&apos;));

?>
```


If you have lots of images inside the images folder, PHP will consume a lot of memory, ergo it is not recommended. I personally think it&apos;s a better idea to loop each image separately:



```
<?php

$image_files = glob(&apos;images/*.JPG&apos;);

foreach ($image_files as $image_file) {
&#xA0; &#xA0; $image = new Imagick($image_file);
&#xA0; &#xA0; // Do something for the image and so on...
}

?>
```


This way only a single image is fitted into the memory at a time.

  

#



on Example #3 Creating a reflection of an image
----------------------------------------------------
/* Clone the image and flip it */
$reflection = $im-&gt;clone();
$reflection-&gt;flipImage();
----------------------------------------------------
it was using the Imagick::clone function

This function has been DEPRECATED as of imagick 3.1.0 in favour of using the clone keyword.

use below code instead:
----------------------------------------------------
/* Clone the image and flip it */
$reflection = clone $im;
$reflection-&gt;flipImage();
----------------------------------------------------

http://php.net/manual/en/imagick.clone.php

  

#

[Official documentation page](https://www.php.net/manual/en/imagick.examples-1.php)

**[To root](/README.md)**