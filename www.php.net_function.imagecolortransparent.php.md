# imagecolortransparent




<div class="phpcode"><span class="html">
I&apos;ve made a very simple script that will retain transparency of images especially when resizing.
<br>
<br>NOTE: Transparency is only supported on GIF and PNG files.
<br>
<br>Parameters:
<br>
<br>$new_image = image resource identifier such as returned by imagecreatetruecolor(). must be passed by reference
<br>$image_source = image resource identifier returned by imagecreatefromjpeg, imagecreatefromgif and imagecreatefrompng. must be passed by reference
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">setTransparency</span><span class="keyword">(</span><span class="default">$new_image</span><span class="keyword">,</span><span class="default">$image_source</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$transparencyIndex </span><span class="keyword">= </span><span class="default">imagecolortransparent</span><span class="keyword">(</span><span class="default">$image_source</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$transparencyColor </span><span class="keyword">= array(</span><span class="string">&apos;red&apos; </span><span class="keyword">=&gt; </span><span class="default">255</span><span class="keyword">, </span><span class="string">&apos;green&apos; </span><span class="keyword">=&gt; </span><span class="default">255</span><span class="keyword">, </span><span class="string">&apos;blue&apos; </span><span class="keyword">=&gt; </span><span class="default">255</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$transparencyIndex </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$transparencyColor&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">imagecolorsforindex</span><span class="keyword">(</span><span class="default">$image_source</span><span class="keyword">, </span><span class="default">$transparencyIndex</span><span class="keyword">);&#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$transparencyIndex&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">imagecolorallocate</span><span class="keyword">(</span><span class="default">$new_image</span><span class="keyword">, </span><span class="default">$transparencyColor</span><span class="keyword">[</span><span class="string">&apos;red&apos;</span><span class="keyword">], </span><span class="default">$transparencyColor</span><span class="keyword">[</span><span class="string">&apos;green&apos;</span><span class="keyword">], </span><span class="default">$transparencyColor</span><span class="keyword">[</span><span class="string">&apos;blue&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">imagefill</span><span class="keyword">(</span><span class="default">$new_image</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$transparencyIndex</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">imagecolortransparent</span><span class="keyword">(</span><span class="default">$new_image</span><span class="keyword">, </span><span class="default">$transparencyIndex</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; } 
<br></span><span class="default">?&gt;
<br></span>
<br>
<br>Sample Usage: (resizing)
<br>
<br><span class="default">&lt;?php
<br>$image_source </span><span class="keyword">= </span><span class="default">imagecreatefrompng</span><span class="keyword">(</span><span class="string">&apos;test.png&apos;</span><span class="keyword">);
<br></span><span class="default">$new_image </span><span class="keyword">= </span><span class="default">imagecreatetruecolor</span><span class="keyword">(</span><span class="default">$width</span><span class="keyword">, </span><span class="default">$height</span><span class="keyword">);
<br></span><span class="default">setTransparency</span><span class="keyword">(</span><span class="default">$new_image</span><span class="keyword">,</span><span class="default">$image_source</span><span class="keyword">);
<br></span><span class="default">imagecopyresampled</span><span class="keyword">(</span><span class="default">$new_image</span><span class="keyword">, </span><span class="default">$image_source</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$new_width</span><span class="keyword">, </span><span class="default">$new_height</span><span class="keyword">, </span><span class="default">$old_width</span><span class="keyword">, </span><span class="default">$old_height</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecolortransparent.php)

**[⬆ to root](/)**