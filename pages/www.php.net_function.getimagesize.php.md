# getimagesize



As noted below, getimagesize will download the entire image before it checks for the requested information. This is extremely slow on large images that are accessed remotely. Since the width/height is in the first few bytes of the file, there is no need to download the entire file. I wrote a function to get the size of a JPEG by streaming bytes until the proper data is found to report the width and height:<br><br>

```
<?php
// Retrieve JPEG width and height without downloading/reading entire image.
function getjpegsize($img_loc) {
    $handle = fopen($img_loc, "rb") or die("Invalid file stream.");
    $new_block = NULL;
    if(!feof($handle)) {
        $new_block = fread($handle, 32);
        $i = 0;
        if($new_block[$i]=="\xFF" &amp;&amp; $new_block[$i+1]=="\xD8" &amp;&amp; $new_block[$i+2]=="\xFF" &amp;&amp; $new_block[$i+3]=="\xE0") {
            $i += 4;
            if($new_block[$i+2]=="\x4A" &amp;&amp; $new_block[$i+3]=="\x46" &amp;&amp; $new_block[$i+4]=="\x49" &amp;&amp; $new_block[$i+5]=="\x46" &amp;&amp; $new_block[$i+6]=="\x00") {
                // Read block size and skip ahead to begin cycling through blocks in search of SOF marker
                $block_size = unpack("H*", $new_block[$i] . $new_block[$i+1]);
                $block_size = hexdec($block_size[1]);
                while(!feof($handle)) {
                    $i += $block_size;
                    $new_block .= fread($handle, $block_size);
                    if($new_block[$i]=="\xFF") {
                        // New block detected, check for SOF marker
                        $sof_marker = array("\xC0", "\xC1", "\xC2", "\xC3", "\xC5", "\xC6", "\xC7", "\xC8", "\xC9", "\xCA", "\xCB", "\xCD", "\xCE", "\xCF");
                        if(in_array($new_block[$i+1], $sof_marker)) {
                            // SOF marker detected. Width and height information is contained in bytes 4-7 after this byte.
                            $size_data = $new_block[$i+2] . $new_block[$i+3] . $new_block[$i+4] . $new_block[$i+5] . $new_block[$i+6] . $new_block[$i+7] . $new_block[$i+8];
                            $unpacked = unpack("H*", $size_data);
                            $unpacked = $unpacked[1];
                            $height = hexdec($unpacked[6] . $unpacked[7] . $unpacked[8] . $unpacked[9]);
                            $width = hexdec($unpacked[10] . $unpacked[11] . $unpacked[12] . $unpacked[13]);
                            return array($width, $height);
                        } else {
                            // Skip block marker and read block size
                            $i += 2;
                            $block_size = unpack("H*", $new_block[$i] . $new_block[$i+1]);
                            $block_size = hexdec($block_size[1]);
                        }
                    } else {
                        return FALSE;
                    }
                }
            }
        }
    }
    return FALSE;
}
?>
```
  

#

If you want to "convert" value returned by "getimagesize()" as index "2" into something more human-readable, you may consider using a function like this one:<br><br>    $imageTypeArray = array<br>    (<br>        0=&gt;&apos;UNKNOWN&apos;,<br>        1=&gt;&apos;GIF&apos;,<br>        2=&gt;&apos;JPEG&apos;,<br>        3=&gt;&apos;PNG&apos;,<br>        4=&gt;&apos;SWF&apos;,<br>        5=&gt;&apos;PSD&apos;,<br>        6=&gt;&apos;BMP&apos;,<br>        7=&gt;&apos;TIFF_II&apos;,<br>        8=&gt;&apos;TIFF_MM&apos;,<br>        9=&gt;&apos;JPC&apos;,<br>        10=&gt;&apos;JP2&apos;,<br>        11=&gt;&apos;JPX&apos;,<br>        12=&gt;&apos;JB2&apos;,<br>        13=&gt;&apos;SWC&apos;,<br>        14=&gt;&apos;IFF&apos;,<br>        15=&gt;&apos;WBMP&apos;,<br>        16=&gt;&apos;XBM&apos;,<br>        17=&gt;&apos;ICO&apos;,<br>        18=&gt;&apos;COUNT&apos;  <br>    );<br>    <br>    $size = getimagesize($filename);<br>    <br>    $size[2] = $imageTypeArray[$size[2]];<br><br>Or something similar.  

#

Note that, if you&apos;re going to be a good programmer and use named constatnts (IMAGETYPE_JPEG) rather than their values (2), you want to use the IMAGETYPE variants - IMAGETYPE_JPEG, IMAGETYPE GIF, IMAGETYPE_PNG, etc.  For some reason, somebody made a horrible decision, and IMG_PNG is actually 4 in my version of PHP, while IMAGETYPE_PNG is 3.  It took me a while to figure out why comparing the type against IMG_PNG was failing...  

#

[Official documentation page](https://www.php.net/manual/en/function.getimagesize.php)

**[To root](/README.md)**