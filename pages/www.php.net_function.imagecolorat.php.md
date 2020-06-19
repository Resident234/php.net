# imagecolorat




<div class="phpcode"><span class="html">
I made a function that calculates the average color of a given image resource and returns it in &quot;#rrggbb&quot; format (hex):<br><br>function average($img) {<br>&#xA0; &#xA0; $w = imagesx($img);<br>&#xA0; &#xA0; $h = imagesy($img);<br>&#xA0; &#xA0; $r = $g = $b = 0;<br>&#xA0; &#xA0; for($y = 0; $y &lt; $h; $y++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; for($x = 0; $x &lt; $w; $x++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rgb = imagecolorat($img, $x, $y);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $r += $rgb &gt;&gt; 16;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $g += $rgb &gt;&gt; 8 &amp; 255;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $b += $rgb &amp; 255;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; $pxls = $w * $h;<br>&#xA0; &#xA0; $r = dechex(round($r / $pxls));<br>&#xA0; &#xA0; $g = dechex(round($g / $pxls));<br>&#xA0; &#xA0; $b = dechex(round($b / $pxls));<br>&#xA0; &#xA0; if(strlen($r) &lt; 2) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $r = 0 . $r;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; if(strlen($g) &lt; 2) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $g = 0 . $g;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; if(strlen($b) &lt; 2) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $b = 0 . $b;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return &quot;#&quot; . $r . $g . $b;<br>}<br><br>Although, I&apos;ve noticed that you can also get a fairly good average color generating a 1px by 1px copy with imagecopyresampled (the pixel generated is colored with the average color).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecolorat.php)

**[To root](/README.md)**