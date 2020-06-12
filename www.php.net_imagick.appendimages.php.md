# Imagick::appendImages




<div class="phpcode"><span class="html">
# How to combine a multi-page pdf file into a single long image:
<br>
<br><span class="default">&lt;?php
<br>$im1 </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">();&#xA0;&#xA0; 
<br></span><span class="default">$im1</span><span class="keyword">-&gt;</span><span class="default">readImage</span><span class="keyword">(</span><span class="string">&apos;multi-page-pdf.pdf&apos;</span><span class="keyword">);
<br></span><span class="default">$im1</span><span class="keyword">-&gt;</span><span class="default">resetIterator</span><span class="keyword">();
<br></span><span class="comment"># Combine multiple images into one, stacked vertically. 
<br></span><span class="default">$ima </span><span class="keyword">= </span><span class="default">$im1</span><span class="keyword">-&gt;</span><span class="default">appendImages</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">$ima</span><span class="keyword">-&gt;</span><span class="default">setImageFormat</span><span class="keyword">(</span><span class="string">&quot;png&quot;</span><span class="keyword">);
<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Content-Type: image/png&quot;</span><span class="keyword">);
<br>echo </span><span class="default">$ima</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.appendimages.php)

**[⬆ to root](/)**