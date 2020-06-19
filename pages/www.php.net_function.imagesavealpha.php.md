# imagesavealpha




<div class="phpcode"><span class="html">
After much trial and error and gnashing of teeth I finally figured out how to composite a png with an 8-bit alpha onto a jpg. This was not obvious to me so I thought I&apos;d share. Hope it helps.
<br>
<br>I&apos;m using this to create a framed thumbnail image:
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// load the frame image (png with 8-bit transparency)
<br></span><span class="default">$frame </span><span class="keyword">= </span><span class="default">imagecreatefrompng</span><span class="keyword">(</span><span class="string">&apos;path/to/frame.png&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// load the thumbnail image
<br></span><span class="default">$thumb </span><span class="keyword">= </span><span class="default">imagecreatefromjpeg</span><span class="keyword">(</span><span class="string">&apos;path/to/thumbnail.jpg&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// get the dimensions of the frame, which we&apos;ll also be using for the
<br>// composited final image.
<br></span><span class="default">$width </span><span class="keyword">= </span><span class="default">imagesx</span><span class="keyword">( </span><span class="default">$frame </span><span class="keyword">);
<br></span><span class="default">$height </span><span class="keyword">= </span><span class="default">imagesy</span><span class="keyword">( </span><span class="default">$frame </span><span class="keyword">);
<br>
<br></span><span class="comment">// create the destination/output image.
<br></span><span class="default">$img</span><span class="keyword">=</span><span class="default">imagecreatetruecolor</span><span class="keyword">( </span><span class="default">$width</span><span class="keyword">, </span><span class="default">$height </span><span class="keyword">);
<br>
<br></span><span class="comment">// enable alpha blending on the destination image.
<br></span><span class="default">imagealphablending</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br>
<br></span><span class="comment">// Allocate a transparent color and fill the new image with it.
<br>// Without this the image will have a black background instead of being transparent.
<br></span><span class="default">$transparent </span><span class="keyword">= </span><span class="default">imagecolorallocatealpha</span><span class="keyword">( </span><span class="default">$img</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">127 </span><span class="keyword">);
<br></span><span class="default">imagefill</span><span class="keyword">( </span><span class="default">$img</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$transparent </span><span class="keyword">);
<br>
<br></span><span class="comment">// copy the thumbnail into the output image.
<br></span><span class="default">imagecopyresampled</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">,</span><span class="default">$thumb</span><span class="keyword">,</span><span class="default">32</span><span class="keyword">,</span><span class="default">30</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">, </span><span class="default">130</span><span class="keyword">, </span><span class="default">100</span><span class="keyword">, </span><span class="default">imagesx</span><span class="keyword">( </span><span class="default">$thumb </span><span class="keyword">), </span><span class="default">imagesy</span><span class="keyword">( </span><span class="default">$thumb </span><span class="keyword">) );
<br>
<br></span><span class="comment">// copy the frame into the output image (layered on top of the thumbnail)
<br></span><span class="default">imagecopyresampled</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">,</span><span class="default">$frame</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">, </span><span class="default">$width</span><span class="keyword">,</span><span class="default">$height</span><span class="keyword">,</span><span class="default">$width</span><span class="keyword">,</span><span class="default">$height</span><span class="keyword">);
<br>
<br></span><span class="default">imagealphablending</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br>
<br></span><span class="comment">// save the alpha
<br></span><span class="default">imagesavealpha</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">);
<br>
<br></span><span class="comment">// emit the image
<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-type: image/png&apos;</span><span class="keyword">);
<br></span><span class="default">imagepng</span><span class="keyword">( </span><span class="default">$img </span><span class="keyword">);
<br>
<br></span><span class="comment">// dispose
<br></span><span class="default">imagedestroy</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">);
<br>
<br></span><span class="comment">// done.
<br></span><span class="keyword">exit;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagesavealpha.php)

**[To root](/README.md)**