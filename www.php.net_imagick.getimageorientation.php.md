# Imagick::getImageOrientation




<div class="phpcode"><span class="html">
Here&apos;s how you can use the getImageOrientation() information to auto-rotate images to their correct orientation...
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// Note: $image is an Imagick object, not a filename! See example use below.
<br></span><span class="keyword">function </span><span class="default">autoRotateImage</span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$orientation </span><span class="keyword">= </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">getImageOrientation</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; switch(</span><span class="default">$orientation</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">imagick</span><span class="keyword">::</span><span class="default">ORIENTATION_BOTTOMRIGHT</span><span class="keyword">: 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">rotateimage</span><span class="keyword">(</span><span class="string">&quot;#000&quot;</span><span class="keyword">, </span><span class="default">180</span><span class="keyword">); </span><span class="comment">// rotate 180 degrees
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">break;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">imagick</span><span class="keyword">::</span><span class="default">ORIENTATION_RIGHTTOP</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">rotateimage</span><span class="keyword">(</span><span class="string">&quot;#000&quot;</span><span class="keyword">, </span><span class="default">90</span><span class="keyword">); </span><span class="comment">// rotate 90 degrees CW
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">break;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">imagick</span><span class="keyword">::</span><span class="default">ORIENTATION_LEFTBOTTOM</span><span class="keyword">: 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">rotateimage</span><span class="keyword">(</span><span class="string">&quot;#000&quot;</span><span class="keyword">, -</span><span class="default">90</span><span class="keyword">); </span><span class="comment">// rotate 90 degrees CCW
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">break;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// Now that it&apos;s auto-rotated, make sure the EXIF data is correct in case the EXIF gets saved with the image!
<br>&#xA0; &#xA0; </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">setImageOrientation</span><span class="keyword">(</span><span class="default">imagick</span><span class="keyword">::</span><span class="default">ORIENTATION_TOPLEFT</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Example use:
<br>
<br><span class="default">&lt;?php
<br>$image </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="string">&apos;my-image-file.jpg&apos;</span><span class="keyword">);
<br></span><span class="default">autoRotateImage</span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">);
<br></span><span class="comment">// - Do other stuff to the image here -
<br></span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">writeImage</span><span class="keyword">(</span><span class="string">&apos;result-image.jpg&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.getimageorientation.php)

**[⬆ to root](/)**