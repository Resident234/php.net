# gzopen



gzopen("php://output","wb") doesn&apos;t work on a web server, nor does fopen("compress.zlib://php://output","wb").<br><br>Here is a snippet to gzip a file and output it on the fly, without using a temporary file, without reading the file into memory, and without reading the file more than once:<br><br>

```
<?php
$fin = fopen($file, "rb");
if ($fin !== FALSE) {
    $fout = fopen("php://output", "wb");
    if ($fout !== FALSE) {
    // write gzip header
    fwrite($fout, "\x1F\x8B\x08\x08".pack("V", filemtime($file))."\0\xFF", 10);
    // write the original file name
    $oname = str_replace("\0", "", basename($file));
    fwrite($fout, $oname."\0", 1+strlen($oname));
    // add the deflate filter using default compression level
    $fltr = stream_filter_append($fout, "zlib.deflate", STREAM_FILTER_WRITE, -1);
    // set up the CRC32 hashing context
    $hctx = hash_init("crc32b");
    // turn off the time limit
    if (!ini_get("safe_mode")) set_time_limit(0);
    $con = TRUE;
    $fsize = 0;
    while (($con !== FALSE) &amp;&amp; !feof($fin)) {
        // deflate works best with buffers &gt;32K
        $con = fread($fin, 64 * 1024);
        if ($con !== FALSE) {
        hash_update($hctx, $con);
        $clen = strlen($con);
        $fsize += $clen;
        fwrite($fout, $con, $clen);
        }
    }
    // remove the deflate filter
    stream_filter_remove($fltr);
    // write the CRC32 value
    // hash_final is a string, not an integer
    $crc = hash_final($hctx, TRUE);
    // need to reverse the hash_final string so it&apos;s little endian
    fwrite($fout, $crc[3].$crc[2].$crc[1].$crc[0], 4);
    // write the original uncompressed file size
    fwrite($fout, pack("V", $fsize), 4);
    fclose($fout);
    }
    fclose($fin);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.gzopen.php)

**[To root](/README.md)**