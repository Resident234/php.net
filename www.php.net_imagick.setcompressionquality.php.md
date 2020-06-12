# Imagick::setCompressionQuality




<div class="phpcode"><span class="html">
A note for people who just couldn&apos;t get this working..
<br>
<br>With PHP 5.1.6, the below works:
<br>
<br><span class="default">&lt;?php
<br>$img</span><span class="keyword">-&gt;</span><span class="default">setCompression</span><span class="keyword">(</span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">COMPRESSION_JPEG</span><span class="keyword">);
<br></span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">setCompressionQuality</span><span class="keyword">(</span><span class="default">80</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>However, with higher versions of PHP (I tried on PHP 5.2.10), the code has no effect (and there are no exceptions or warnings thrown by Imagick as well).
<br>
<br>The code that works instead is:
<br>
<br><span class="default">&lt;?php
<br>$img</span><span class="keyword">-&gt;</span><span class="default">setImageCompression</span><span class="keyword">(</span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">COMPRESSION_JPEG</span><span class="keyword">);
<br></span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">setImageCompressionQuality</span><span class="keyword">(</span><span class="default">80</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>and this is backwards compatible (Works on PHP 5.1.6 as well as 5.2.10)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setcompressionquality.php)

**[⬆ to root](/)**