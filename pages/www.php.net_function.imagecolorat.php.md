# imagecolorat





I made a function that calculates the average color of a given image resource and returns it in &quot;#rrggbb&quot; format (hex):

function average($img) {
&#xA0; &#xA0; $w = imagesx($img);
&#xA0; &#xA0; $h = imagesy($img);
&#xA0; &#xA0; $r = $g = $b = 0;
&#xA0; &#xA0; for($y = 0; $y &lt; $h; $y++) {
&#xA0; &#xA0; &#xA0; &#xA0; for($x = 0; $x &lt; $w; $x++) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rgb = imagecolorat($img, $x, $y);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $r += $rgb &gt;&gt; 16;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $g += $rgb &gt;&gt; 8 &amp; 255;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $b += $rgb &amp; 255;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; $pxls = $w * $h;
&#xA0; &#xA0; $r = dechex(round($r / $pxls));
&#xA0; &#xA0; $g = dechex(round($g / $pxls));
&#xA0; &#xA0; $b = dechex(round($b / $pxls));
&#xA0; &#xA0; if(strlen($r) &lt; 2) {
&#xA0; &#xA0; &#xA0; &#xA0; $r = 0 . $r;
&#xA0; &#xA0; }
&#xA0; &#xA0; if(strlen($g) &lt; 2) {
&#xA0; &#xA0; &#xA0; &#xA0; $g = 0 . $g;
&#xA0; &#xA0; }
&#xA0; &#xA0; if(strlen($b) &lt; 2) {
&#xA0; &#xA0; &#xA0; &#xA0; $b = 0 . $b;
&#xA0; &#xA0; }
&#xA0; &#xA0; return &quot;#&quot; . $r . $g . $b;
}

Although, I&apos;ve noticed that you can also get a fairly good average color generating a 1px by 1px copy with imagecopyresampled (the pixel generated is colored with the average color).

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecolorat.php)

**[To root](/README.md)**