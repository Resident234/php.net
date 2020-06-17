# imagecreatefromjpeg




<div class="phpcode"><span class="html">
This function does not honour EXIF orientation data.&#xA0; Pictures that are rotated using EXIF, will show up in the original orientation after being handled by imagecreatefromjpeg().&#xA0; Below is a function to create an image from JPEG while honouring EXIF orientation data.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">imagecreatefromjpegexif</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$img </span><span class="keyword">= </span><span class="default">imagecreatefromjpeg</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$exif </span><span class="keyword">= </span><span class="default">exif_read_data</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$img </span><span class="keyword">&amp;&amp; </span><span class="default">$exif </span><span class="keyword">&amp;&amp; isset(</span><span class="default">$exif</span><span class="keyword">[</span><span class="string">&apos;Orientation&apos;</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ort </span><span class="keyword">= </span><span class="default">$exif</span><span class="keyword">[</span><span class="string">&apos;Orientation&apos;</span><span class="keyword">];<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$ort </span><span class="keyword">== </span><span class="default">6 </span><span class="keyword">|| </span><span class="default">$ort </span><span class="keyword">== </span><span class="default">5</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$img </span><span class="keyword">= </span><span class="default">imagerotate</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">270</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$ort </span><span class="keyword">== </span><span class="default">3 </span><span class="keyword">|| </span><span class="default">$ort </span><span class="keyword">== </span><span class="default">4</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$img </span><span class="keyword">= </span><span class="default">imagerotate</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">180</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$ort </span><span class="keyword">== </span><span class="default">8 </span><span class="keyword">|| </span><span class="default">$ort </span><span class="keyword">== </span><span class="default">7</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$img </span><span class="keyword">= </span><span class="default">imagerotate</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">90</span><span class="keyword">, </span><span class="default">null</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$ort </span><span class="keyword">== </span><span class="default">5 </span><span class="keyword">|| </span><span class="default">$ort </span><span class="keyword">== </span><span class="default">4 </span><span class="keyword">|| </span><span class="default">$ort </span><span class="keyword">== </span><span class="default">7</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">imageflip</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">IMG_FLIP_HORIZONTAL</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$img</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This little function allows you to create an image based on the popular image types without worrying about what it is:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">imageCreateFromAny</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$type </span><span class="keyword">= </span><span class="default">exif_imagetype</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">); </span><span class="comment">// [] if you don&apos;t have exif you could use getImageSize()
<br>&#xA0; &#xA0; </span><span class="default">$allowedTypes </span><span class="keyword">= array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">1</span><span class="keyword">,&#xA0; </span><span class="comment">// [] gif
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">2</span><span class="keyword">,&#xA0; </span><span class="comment">// [] jpg
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">3</span><span class="keyword">,&#xA0; </span><span class="comment">// [] png
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">6&#xA0;&#xA0; </span><span class="comment">// [] bmp
<br>&#xA0; &#xA0; </span><span class="keyword">);
<br>&#xA0; &#xA0; if (!</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$type</span><span class="keyword">, </span><span class="default">$allowedTypes</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; switch (</span><span class="default">$type</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">1 </span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$im </span><span class="keyword">= </span><span class="default">imageCreateFromGif</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">2 </span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$im </span><span class="keyword">= </span><span class="default">imageCreateFromJpeg</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">3 </span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$im </span><span class="keyword">= </span><span class="default">imageCreateFromPng</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">6 </span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$im </span><span class="keyword">= </span><span class="default">imageCreateFromBmp</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; }&#xA0; &#xA0; 
<br>&#xA0; &#xA0; return </span><span class="default">$im</span><span class="keyword">;&#xA0; 
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromjpeg.php)

**[To root](/README.md)**