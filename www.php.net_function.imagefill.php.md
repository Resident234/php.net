# imagefill




<div class="phpcode"><span class="html">
Creating a new true-color image, filling it with a transparent color, and saving it as a PNG image can be accomplished with the following:<br><br><span class="default">&lt;?php<br><br>$new&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">imagecreatetruecolor</span><span class="keyword">(</span><span class="default">320</span><span class="keyword">, </span><span class="default">320</span><span class="keyword">);<br></span><span class="default">$color </span><span class="keyword">= </span><span class="default">imagecolorallocatealpha</span><span class="keyword">(</span><span class="default">$new</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">127</span><span class="keyword">);<br></span><span class="default">imagefill</span><span class="keyword">(</span><span class="default">$new</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$color</span><span class="keyword">);<br></span><span class="default">imagesavealpha</span><span class="keyword">(</span><span class="default">$new</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">); </span><span class="comment">// it took me a good 10 minutes to figure this part out<br></span><span class="default">imagepng</span><span class="keyword">(</span><span class="default">$new</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>The image needs to be created with imagecreatetruecolor(), you must use imagefill() instead of imagefilledrectange(), and you need to call imagesavealpha(). No other combination of functions calls seems to produce the intended results.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagefill.php)

**[To root](/README.md)**