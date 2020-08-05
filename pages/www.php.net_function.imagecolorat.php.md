# imagecolorat



I made a function that calculates the average color of a given image resource and returns it in "#rrggbb" format (hex):<br><br>function average($img) {<br>    $w = imagesx($img);<br>    $h = imagesy($img);<br>    $r = $g = $b = 0;<br>    for($y = 0; $y &lt; $h; $y++) {<br>        for($x = 0; $x &lt; $w; $x++) {<br>            $rgb = imagecolorat($img, $x, $y);<br>            $r += $rgb &gt;&gt; 16;<br>            $g += $rgb &gt;&gt; 8 &amp; 255;<br>            $b += $rgb &amp; 255;<br>        }<br>    }<br>    $pxls = $w * $h;<br>    $r = dechex(round($r / $pxls));<br>    $g = dechex(round($g / $pxls));<br>    $b = dechex(round($b / $pxls));<br>    if(strlen($r) &lt; 2) {<br>        $r = 0 . $r;<br>    }<br>    if(strlen($g) &lt; 2) {<br>        $g = 0 . $g;<br>    }<br>    if(strlen($b) &lt; 2) {<br>        $b = 0 . $b;<br>    }<br>    return "#" . $r . $g . $b;<br>}<br><br>Although, I&apos;ve noticed that you can also get a fairly good average color generating a 1px by 1px copy with imagecopyresampled (the pixel generated is colored with the average color).  

---

[Official documentation page](https://www.php.net/manual/en/function.imagecolorat.php)

**[To root](/README.md)**