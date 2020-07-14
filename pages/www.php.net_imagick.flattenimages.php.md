# Imagick::flattenImages



Replying to Francois:<br>

```
<?php
$im-&gt;setImageBackgroundColor(&apos;white&apos;);
$im-&gt;setImageAlphaChannel(Imagick::ALPHACHANNEL_REMOVE);
$im-&gt;mergeImageLayers(Imagick::LAYERMETHOD_FLATTEN);
?>
```


Imagick::ALPHACHANNEL_REMOVE has been added in 3.2.0b2 (before the RC): http://pecl.php.net/package-info.php?package=imagick&amp;version=3.2.0b2

The problem is with people who want to implement this, but are stuck with an Imagick version &lt; 3.2.0b2. They can&apos;t use this constant. However, all is not lost. I&apos;ve found a reference where someone got this to work using an integer: http://stackoverflow.com/q/28154179/1000608

The number he used is 11, which seems to suggest that the value of Imagick::ALPHACHANNEL_REMOVE is 11, and the function worked correctly in this usecase even before the constant was implemented. So if you&apos;re stuck with &lt;3.2.0b2 this is the code you need:



```
<?php
$im-&gt;setImageBackgroundColor(&apos;white&apos;);
$im-&gt;setImageAlphaChannel(11);
$im-&gt;mergeImageLayers(Imagick::LAYERMETHOD_FLATTEN);
?>
```
<br><br>This works at least as far back as version 3.1.0~rc1-1 (current version of the php5-imagick package in Debian 7).  

#

The actual replacement now that flattenImages() has been deprecated is:<br><br>

```
<?php
$im = $im-&gt;mergeImageLayers(Imagick::LAYERMETHOD_FLATTEN);
?>
```
<br><br>So you need to (re)assign the returned Imagick object.  

#

[Official documentation page](https://www.php.net/manual/en/imagick.flattenimages.php)

**[To root](/README.md)**