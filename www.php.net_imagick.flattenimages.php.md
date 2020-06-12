# Imagick::flattenImages




<div class="phpcode"><span class="html">
Replying to Francois:<br><span class="default">&lt;?php<br>$im</span><span class="keyword">-&gt;</span><span class="default">setImageBackgroundColor</span><span class="keyword">(</span><span class="string">&apos;white&apos;</span><span class="keyword">);<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">setImageAlphaChannel</span><span class="keyword">(</span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">ALPHACHANNEL_REMOVE</span><span class="keyword">);<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">mergeImageLayers</span><span class="keyword">(</span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">LAYERMETHOD_FLATTEN</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Imagick::ALPHACHANNEL_REMOVE has been added in 3.2.0b2 (before the RC): <a href="http://pecl.php.net/package-info.php?package=imagick&amp;version=3.2.0b2" rel="nofollow" target="_blank">http://pecl.php.net/package-info.php?package=imagick&amp;version=3.2.0b2</a><br><br>The problem is with people who want to implement this, but are stuck with an Imagick version &lt; 3.2.0b2. They can&apos;t use this constant. However, all is not lost. I&apos;ve found a reference where someone got this to work using an integer: <a href="http://stackoverflow.com/q/28154179/1000608" rel="nofollow" target="_blank">http://stackoverflow.com/q/28154179/1000608</a><br><br>The number he used is 11, which seems to suggest that the value of Imagick::ALPHACHANNEL_REMOVE is 11, and the function worked correctly in this usecase even before the constant was implemented. So if you&apos;re stuck with &lt;3.2.0b2 this is the code you need:<br><br><span class="default">&lt;?php<br>$im</span><span class="keyword">-&gt;</span><span class="default">setImageBackgroundColor</span><span class="keyword">(</span><span class="string">&apos;white&apos;</span><span class="keyword">);<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">setImageAlphaChannel</span><span class="keyword">(</span><span class="default">11</span><span class="keyword">);<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">mergeImageLayers</span><span class="keyword">(</span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">LAYERMETHOD_FLATTEN</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This works at least as far back as version 3.1.0~rc1-1 (current version of the php5-imagick package in Debian 7).</span>
</div>
  

#


<div class="phpcode"><span class="html">
The actual replacement now that flattenImages() has been deprecated is:<br><br><span class="default">&lt;?php<br>$im </span><span class="keyword">= </span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">mergeImageLayers</span><span class="keyword">(</span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">LAYERMETHOD_FLATTEN</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>So you need to (re)assign the returned Imagick object.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.flattenimages.php)

**[⬆ to root](/)**