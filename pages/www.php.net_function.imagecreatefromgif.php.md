# imagecreatefromgif





An updated is_ani based on issues reported here and elsewhere



```
<?php
function is_ani($filename) {
&#xA0; &#xA0; if(!($fh = @fopen($filename, &apos;rb&apos;)))
&#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; $count = 0;
&#xA0; &#xA0; //an animated gif contains multiple &quot;frames&quot;, with each frame having a 
&#xA0; &#xA0; //header made up of:
&#xA0; &#xA0; // * a static 4-byte sequence (\x00\x21\xF9\x04)
&#xA0; &#xA0; // * 4 variable bytes
&#xA0; &#xA0; // * a static 2-byte sequence (\x00\x2C) (some variants may use \x00\x21 ?)
&#xA0; &#xA0; 
&#xA0; &#xA0; // We read through the file til we reach the end of the file, or we&apos;ve found 
&#xA0; &#xA0; // at least 2 frame headers
&#xA0; &#xA0; while(!feof($fh) &amp;&amp; $count &lt; 2) {
&#xA0; &#xA0; &#xA0; &#xA0; $chunk = fread($fh, 1024 * 100); //read 100kb at a time
&#xA0; &#xA0; &#xA0; &#xA0; $count += preg_match_all(&apos;#\x00\x21\xF9\x04.{4}\x00(\x2C|\x21)#s&apos;, $chunk, $matches);
&#xA0;&#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; fclose($fh);
&#xA0; &#xA0; return $count &gt; 1;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromgif.php)

**[To root](/README.md)**