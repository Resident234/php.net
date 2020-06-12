# Imagick::trimImage




<div class="phpcode"><span class="html">
After operations that change the crop of the image, like trimImage does, IM preserves the old canvas and positioning info. If you need to do additional operations on the image based on the new size, you&apos;ll need to reset this info with setImagePage. This is the equivalent of the +repage command line argument.
<br>
<br><span class="default">&lt;?php
<br>$im</span><span class="keyword">-&gt;</span><span class="default">trimImage</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">setImagePage</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.trimimage.php)

**[⬆ to root](/)**