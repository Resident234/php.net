# Imagick::readImage




<div class="phpcode"><span class="html">
Use this to convert all pages of a PDF to JPG:
<br>
<br><span class="default">&lt;?php
<br>$imagick </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">();
<br></span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">readImage</span><span class="keyword">(</span><span class="string">&apos;myfile.pdf&apos;</span><span class="keyword">);
<br></span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">writeImages</span><span class="keyword">(</span><span class="string">&apos;converted.jpg&apos;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>If you need better quality, try adding $imagick-&gt;setResolution(150, 150); before reading the file!
<br>
<br>If you experience transparency problems when converting PDF to JPEG (black background), try flattening your file:
<br>
<br><span class="default">&lt;?php
<br>$imagick </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">();
<br></span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">readImage</span><span class="keyword">(</span><span class="string">&apos;myfile.pdf[0]&apos;</span><span class="keyword">);
<br></span><span class="default">$imagick </span><span class="keyword">= </span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">flattenImages</span><span class="keyword">();
<br></span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">writeFile</span><span class="keyword">(</span><span class="string">&apos;pageone.jpg&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>In order to read pages from a PDF-file use [PAGENUMBER] after the filename (pages start from zero!).
<br>
<br>Example: Read page #1 from test.pdf
<br>
<br><span class="default">&lt;?php
<br>$imagick </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">();
<br></span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">readImage</span><span class="keyword">(</span><span class="string">&apos;test.pdf[0]&apos;</span><span class="keyword">);
<br></span><span class="default">$imagick</span><span class="keyword">-&gt;</span><span class="default">writeImage</span><span class="keyword">(</span><span class="string">&apos;page_one.jpg&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.readimage.php)

**[⬆ to root](/)**