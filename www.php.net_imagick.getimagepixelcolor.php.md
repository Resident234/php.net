# Imagick::getImagePixelColor




<div class="phpcode"><span class="html">
I&apos;m sure there are a lot of people like me who have been wondering, &quot;How you manage to produce a human readable output of this operation?&quot;
<br>
<br><span class="default">&lt;?php
<br>$image </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="string">&apos;testimage.jpg&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; 
<br></span><span class="default">$y </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$pixel </span><span class="keyword">= </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">getImagePixelColor</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">, </span><span class="default">$y</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>If you try to print an output of the $pixel object, you get nothing. You have to use one of the ImagickPixel operations to get back a value.
<br>
<br>You can do either of the following:
<br>
<br><span class="default">&lt;?php
<br>$colors </span><span class="keyword">= </span><span class="default">$pixel</span><span class="keyword">-&gt;</span><span class="default">getColor</span><span class="keyword">();
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$colors</span><span class="keyword">); </span><span class="comment">// produces Array([r]=&gt;255,[g]=&gt;255,[b]=&gt;255,[a]=&gt;1);
<br>
<br></span><span class="default">$pixel</span><span class="keyword">-&gt;</span><span class="default">getColorAsString</span><span class="keyword">(); </span><span class="comment">// produces rgb(255,255,255);
<br></span><span class="default">?&gt;
<br></span>
<br>The place where I was getting hung up was how to get the data that was captured in the Imagick::getImagePixelColor operation into an ImagickPixel object. I was trying to find ways of passing the value to a newly instantiated ImagickPixel object. Well, it appears that once you&apos;ve captured your color data using Imagick::getImagePixelColor, what&apos;s returned IS an ImagickPixel object!
<br>
<br>As a further note, you do not need to convert this to a human readable format if you just want to take a color sample at a single point on your image to plug into another operation. 
<br>
<br>For example, if you wanted to perform a flood fill effect on a certain color you could plug in the instance of the ImagickPixel object directly. 
<br>
<br>The following fill perform a flood fill effect at coordinates 1,1 on your image using Green as the fill color and the color sampled at 1,1 as the target color to fill.
<br>
<br><span class="default">&lt;?php
<br>$hexcolor </span><span class="keyword">= </span><span class="string">&apos;#00ff00&apos;</span><span class="keyword">;
<br></span><span class="default">$fuzz </span><span class="keyword">= </span><span class="string">&apos;4000&apos;</span><span class="keyword">;
<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$y </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$pixel </span><span class="keyword">= </span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">getImagePixelColor</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">, </span><span class="default">$y</span><span class="keyword">);
<br></span><span class="default">$image</span><span class="keyword">-&gt;</span><span class="default">floodfillPaintImage</span><span class="keyword">(</span><span class="default">$hexcolor</span><span class="keyword">, </span><span class="default">$fuzz</span><span class="keyword">, </span><span class="default">$pixel</span><span class="keyword">, </span><span class="default">$x</span><span class="keyword">, </span><span class="default">$y</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.getimagepixelcolor.php)

**[⬆ to root](/)**