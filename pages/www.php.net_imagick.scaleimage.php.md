# Imagick::scaleImage





If anyone finds &quot;The other parameter will be calculated if 0 is passed as either param. &quot; to be a bit confusing, it means approximately this:



```
<?php
$im = new Imagick(&apos;example.jpg&apos;);
$im-&gt;scaleImage(300, 0);
?>
```


This scales the image such that it is now 300 pixels wide, and automatically calculates the height to keep the image at the same aspect ratio.



```
<?php
$im = new Imagick(&apos;example.jpg&apos;);
$im-&gt;scaleImage(0, 300);
?>
```


Similarly, this example scales the image to make it 300 pixels tall, and the method automatically recalculates the image&apos;s height to maintain the aspect ratio.

  

#

[Official documentation page](https://www.php.net/manual/en/imagick.scaleimage.php)

**[To root](/README.md)**