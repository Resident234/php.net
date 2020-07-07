# getimagesize





As noted below, getimagesize will download the entire image before it checks for the requested information. This is extremely slow on large images that are accessed remotely. Since the width/height is in the first few bytes of the file, there is no need to download the entire file. I wrote a function to get the size of a JPEG by streaming bytes until the proper data is found to report the width and height:



```
<?php
// Retrieve JPEG width and height without downloading/reading entire image.
function getjpegsize($img_loc) {
&#xA0; &#xA0; $handle = fopen($img_loc, &quot;rb&quot;) or die(&quot;Invalid file stream.&quot;);
&#xA0; &#xA0; $new_block = NULL;
&#xA0; &#xA0; if(!feof($handle)) {
&#xA0; &#xA0; &#xA0; &#xA0; $new_block = fread($handle, 32);
&#xA0; &#xA0; &#xA0; &#xA0; $i = 0;
&#xA0; &#xA0; &#xA0; &#xA0; if($new_block[$i]==&quot;\xFF&quot; &amp;&amp; $new_block[$i+1]==&quot;\xD8&quot; &amp;&amp; $new_block[$i+2]==&quot;\xFF&quot; &amp;&amp; $new_block[$i+3]==&quot;\xE0&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i += 4;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($new_block[$i+2]==&quot;\x4A&quot; &amp;&amp; $new_block[$i+3]==&quot;\x46&quot; &amp;&amp; $new_block[$i+4]==&quot;\x49&quot; &amp;&amp; $new_block[$i+5]==&quot;\x46&quot; &amp;&amp; $new_block[$i+6]==&quot;\x00&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Read block size and skip ahead to begin cycling through blocks in search of SOF marker
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $block_size = unpack(&quot;H*&quot;, $new_block[$i] . $new_block[$i+1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $block_size = hexdec($block_size[1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while(!feof($handle)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i += $block_size;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $new_block .= fread($handle, $block_size);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($new_block[$i]==&quot;\xFF&quot;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // New block detected, check for SOF marker
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $sof_marker = array(&quot;\xC0&quot;, &quot;\xC1&quot;, &quot;\xC2&quot;, &quot;\xC3&quot;, &quot;\xC5&quot;, &quot;\xC6&quot;, &quot;\xC7&quot;, &quot;\xC8&quot;, &quot;\xC9&quot;, &quot;\xCA&quot;, &quot;\xCB&quot;, &quot;\xCD&quot;, &quot;\xCE&quot;, &quot;\xCF&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(in_array($new_block[$i+1], $sof_marker)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // SOF marker detected. Width and height information is contained in bytes 4-7 after this byte.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $size_data = $new_block[$i+2] . $new_block[$i+3] . $new_block[$i+4] . $new_block[$i+5] . $new_block[$i+6] . $new_block[$i+7] . $new_block[$i+8];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $unpacked = unpack(&quot;H*&quot;, $size_data);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $unpacked = $unpacked[1];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $height = hexdec($unpacked[6] . $unpacked[7] . $unpacked[8] . $unpacked[9]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $width = hexdec($unpacked[10] . $unpacked[11] . $unpacked[12] . $unpacked[13]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return array($width, $height);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Skip block marker and read block size
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i += 2;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $block_size = unpack(&quot;H*&quot;, $new_block[$i] . $new_block[$i+1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $block_size = hexdec($block_size[1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return FALSE;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; return FALSE;
}
?>
```



  

#



If you want to &quot;convert&quot; value returned by &quot;getimagesize()&quot; as index &quot;2&quot; into something more human-readable, you may consider using a function like this one:

&#xA0; &#xA0; $imageTypeArray = array
&#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; 0=&gt;&apos;UNKNOWN&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 1=&gt;&apos;GIF&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 2=&gt;&apos;JPEG&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 3=&gt;&apos;PNG&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 4=&gt;&apos;SWF&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 5=&gt;&apos;PSD&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 6=&gt;&apos;BMP&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 7=&gt;&apos;TIFF_II&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 8=&gt;&apos;TIFF_MM&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 9=&gt;&apos;JPC&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 10=&gt;&apos;JP2&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 11=&gt;&apos;JPX&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 12=&gt;&apos;JB2&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 13=&gt;&apos;SWC&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 14=&gt;&apos;IFF&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 15=&gt;&apos;WBMP&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 16=&gt;&apos;XBM&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 17=&gt;&apos;ICO&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; 18=&gt;&apos;COUNT&apos;&#xA0; 
&#xA0; &#xA0; );
&#xA0; &#xA0; 
&#xA0; &#xA0; $size = getimagesize($filename);
&#xA0; &#xA0; 
&#xA0; &#xA0; $size[2] = $imageTypeArray[$size[2]];

Or something similar.

  

#



Note that, if you&apos;re going to be a good programmer and use named constatnts (IMAGETYPE_JPEG) rather than their values (2), you want to use the IMAGETYPE variants - IMAGETYPE_JPEG, IMAGETYPE GIF, IMAGETYPE_PNG, etc.&#xA0; For some reason, somebody made a horrible decision, and IMG_PNG is actually 4 in my version of PHP, while IMAGETYPE_PNG is 3.&#xA0; It took me a while to figure out why comparing the type against IMG_PNG was failing...

  

#

[Official documentation page](https://www.php.net/manual/en/function.getimagesize.php)

**[To root](/README.md)**