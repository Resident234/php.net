# Imagick::coalesceImages




<div class="phpcode"><span class="html">
resize and/or crop an animated GIF
<br>
<br><span class="default">&lt;?php
<br>$image </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="default">$file_src</span><span class="keyword">);
<br>
<br></span><span class="default">$image </span><span class="keyword">= </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">coalesceImages</span><span class="keyword">();
<br>
<br>foreach (</span><span class="default">$image </span><span class="keyword">as </span><span class="default">$frame</span><span class="keyword">) {
<br>&#xA0; </span><span class="default">$frame</span><span class="keyword">-&gt;</span><span class="default">cropImage</span><span class="keyword">(</span><span class="default">$crop_w</span><span class="keyword">, </span><span class="default">$crop_h</span><span class="keyword">, </span><span class="default">$crop_x</span><span class="keyword">, </span><span class="default">$crop_y</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$frame</span><span class="keyword">-&gt;</span><span class="default">thumbnailImage</span><span class="keyword">(</span><span class="default">$size_w</span><span class="keyword">, </span><span class="default">$size_h</span><span class="keyword">);
<br>&#xA0; </span><span class="default">$frame</span><span class="keyword">-&gt;</span><span class="default">setImagePage</span><span class="keyword">(</span><span class="default">$size_w</span><span class="keyword">, </span><span class="default">$size_h</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">$image </span><span class="keyword">= </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">deconstructImages</span><span class="keyword">();
<br></span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">writeImages</span><span class="keyword">(</span><span class="default">$file_dst</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.coalesceimages.php)

**[To root](/README.md)**