# imagecopymerge




<div class="phpcode"><span class="html">
I&apos;ve just checked PHP&apos;s issue tracker and a core developer says that this function was never meant to support alpha channel! and they refused to commit the provided patch! so ridicules 
<br>Anyway i tried rodrigo&apos;s workaround and it worked quite well, thanks rodrigo for sharing it.
<br>I came up with another idea which is much faster than his solution, (it may need a little bit more memory)
<br>
<br>Hope it helps someone.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * PNG ALPHA CHANNEL SUPPORT for imagecopymerge();
<br> * by Sina Salek
<br> *
<br> * Bugfix by Ralph Voigt (bug which causes it
<br> * to work only for $src_x = $src_y = 0. 
<br> * Also, inverting opacity is not necessary.)
<br> * 08-JAN-2011
<br> *
<br> **/
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">imagecopymerge_alpha</span><span class="keyword">(</span><span class="default">$dst_im</span><span class="keyword">, </span><span class="default">$src_im</span><span class="keyword">, </span><span class="default">$dst_x</span><span class="keyword">, </span><span class="default">$dst_y</span><span class="keyword">, </span><span class="default">$src_x</span><span class="keyword">, </span><span class="default">$src_y</span><span class="keyword">, </span><span class="default">$src_w</span><span class="keyword">, </span><span class="default">$src_h</span><span class="keyword">, </span><span class="default">$pct</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// creating a cut resource
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cut </span><span class="keyword">= </span><span class="default">imagecreatetruecolor</span><span class="keyword">(</span><span class="default">$src_w</span><span class="keyword">, </span><span class="default">$src_h</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// copying relevant section from background to the cut resource
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">imagecopy</span><span class="keyword">(</span><span class="default">$cut</span><span class="keyword">, </span><span class="default">$dst_im</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$dst_x</span><span class="keyword">, </span><span class="default">$dst_y</span><span class="keyword">, </span><span class="default">$src_w</span><span class="keyword">, </span><span class="default">$src_h</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// copying relevant section from watermark to the cut resource
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">imagecopy</span><span class="keyword">(</span><span class="default">$cut</span><span class="keyword">, </span><span class="default">$src_im</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$src_x</span><span class="keyword">, </span><span class="default">$src_y</span><span class="keyword">, </span><span class="default">$src_w</span><span class="keyword">, </span><span class="default">$src_h</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// insert cut resource to destination image
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">imagecopymerge</span><span class="keyword">(</span><span class="default">$dst_im</span><span class="keyword">, </span><span class="default">$cut</span><span class="keyword">, </span><span class="default">$dst_x</span><span class="keyword">, </span><span class="default">$dst_y</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$src_w</span><span class="keyword">, </span><span class="default">$src_h</span><span class="keyword">, </span><span class="default">$pct</span><span class="keyword">);
<br>&#xA0; &#xA0; } 
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
imagecopymerge PHP function helped me to create a fast method that replaces one color by another in true color images.<br><br>Beforehand for this purpose I had to loop over all the pixels of an image and replace color pixel by pixel using imagecolorat and imagesetpixel; this method is really slow for large images.<br><br>So here is the fast one:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">replaceColorInImage </span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">, </span><span class="default">$old_r</span><span class="keyword">, </span><span class="default">$old_g</span><span class="keyword">, </span><span class="default">$old_b</span><span class="keyword">, </span><span class="default">$new_r</span><span class="keyword">, </span><span class="default">$new_g</span><span class="keyword">, </span><span class="default">$new_b</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">imagecolortransparent </span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">, </span><span class="default">imagecolorallocate </span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">, </span><span class="default">$old_r</span><span class="keyword">, </span><span class="default">$old_g</span><span class="keyword">, </span><span class="default">$old_b</span><span class="keyword">));<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$w </span><span class="keyword">= </span><span class="default">imagesx </span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$h </span><span class="keyword">= </span><span class="default">imagesy </span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$resImage </span><span class="keyword">= </span><span class="default">imagecreatetruecolor </span><span class="keyword">(</span><span class="default">$w</span><span class="keyword">, </span><span class="default">$h</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">imagefill </span><span class="keyword">(</span><span class="default">$resImage</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">imagecolorallocate </span><span class="keyword">(</span><span class="default">$resImage</span><span class="keyword">, </span><span class="default">$new_r</span><span class="keyword">, </span><span class="default">$new_g</span><span class="keyword">, </span><span class="default">$new_b</span><span class="keyword">));<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">imagecopymerge </span><span class="keyword">(</span><span class="default">$resImage</span><span class="keyword">, </span><span class="default">$image</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$w</span><span class="keyword">, </span><span class="default">$h</span><span class="keyword">, </span><span class="default">100</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="default">$resImage</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecopymerge.php)

**[⬆ to root](/)**