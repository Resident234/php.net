# Imagick::cropImage




<div class="phpcode"><span class="html">
When cropping gif-images (I had no problems with jpg and png images), the canvas is not removed. Please run the following command on the cropped gif, to remove the blank space:<br><br>$im-&gt;setImagePage(0, 0, 0, 0);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.cropimage.php)

**[⬆ to root](/)**