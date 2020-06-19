# Imagick::getImageSize




<div class="phpcode"><span class="html">
Practical use to get the dimensions of the image:
<br>
<br><span class="default">&lt;?php
<br>$image </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="default">$image_src</span><span class="keyword">);
<br></span><span class="default">$d </span><span class="keyword">= </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">getImageGeometry</span><span class="keyword">();
<br></span><span class="default">$w </span><span class="keyword">= </span><span class="default">$d</span><span class="keyword">[</span><span class="string">&apos;width&apos;</span><span class="keyword">];
<br></span><span class="default">$h </span><span class="keyword">= </span><span class="default">$d</span><span class="keyword">[</span><span class="string">&apos;height&apos;</span><span class="keyword">];
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.getimagesize.php)

**[To root](/README.md)**