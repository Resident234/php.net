# gzopen





gzopen(&quot;php://output&quot;,&quot;wb&quot;) doesn&apos;t work on a web server, nor does fopen(&quot;compress.zlib://php://output&quot;,&quot;wb&quot;).

Here is a snippet to gzip a file and output it on the fly, without using a temporary file, without reading the file into memory, and without reading the file more than once:



```
<?php
$fin = fopen($file, &quot;rb&quot;);
if ($fin !== FALSE) {
&#xA0; &#xA0; $fout = fopen(&quot;php://output&quot;, &quot;wb&quot;);
&#xA0; &#xA0; if ($fout !== FALSE) {
&#xA0; &#xA0; // write gzip header
&#xA0; &#xA0; fwrite($fout, &quot;\x1F\x8B\x08\x08&quot;.pack(&quot;V&quot;, filemtime($file)).&quot;\0\xFF&quot;, 10);
&#xA0; &#xA0; // write the original file name
&#xA0; &#xA0; $oname = str_replace(&quot;\0&quot;, &quot;&quot;, basename($file));
&#xA0; &#xA0; fwrite($fout, $oname.&quot;\0&quot;, 1+strlen($oname));
&#xA0; &#xA0; // add the deflate filter using default compression level
&#xA0; &#xA0; $fltr = stream_filter_append($fout, &quot;zlib.deflate&quot;, STREAM_FILTER_WRITE, -1);
&#xA0; &#xA0; // set up the CRC32 hashing context
&#xA0; &#xA0; $hctx = hash_init(&quot;crc32b&quot;);
&#xA0; &#xA0; // turn off the time limit
&#xA0; &#xA0; if (!ini_get(&quot;safe_mode&quot;)) set_time_limit(0);
&#xA0; &#xA0; $con = TRUE;
&#xA0; &#xA0; $fsize = 0;
&#xA0; &#xA0; while (($con !== FALSE) &amp;&amp; !feof($fin)) {
&#xA0; &#xA0; &#xA0; &#xA0; // deflate works best with buffers &gt;32K
&#xA0; &#xA0; &#xA0; &#xA0; $con = fread($fin, 64 * 1024);
&#xA0; &#xA0; &#xA0; &#xA0; if ($con !== FALSE) {
&#xA0; &#xA0; &#xA0; &#xA0; hash_update($hctx, $con);
&#xA0; &#xA0; &#xA0; &#xA0; $clen = strlen($con);
&#xA0; &#xA0; &#xA0; &#xA0; $fsize += $clen;
&#xA0; &#xA0; &#xA0; &#xA0; fwrite($fout, $con, $clen);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; // remove the deflate filter
&#xA0; &#xA0; stream_filter_remove($fltr);
&#xA0; &#xA0; // write the CRC32 value
&#xA0; &#xA0; // hash_final is a string, not an integer
&#xA0; &#xA0; $crc = hash_final($hctx, TRUE);
&#xA0; &#xA0; // need to reverse the hash_final string so it&apos;s little endian
&#xA0; &#xA0; fwrite($fout, $crc[3].$crc[2].$crc[1].$crc[0], 4);
&#xA0; &#xA0; // write the original uncompressed file size
&#xA0; &#xA0; fwrite($fout, pack(&quot;V&quot;, $fsize), 4);
&#xA0; &#xA0; fclose($fout);
&#xA0; &#xA0; }
&#xA0; &#xA0; fclose($fin);
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.gzopen.php)

**[To root](/README.md)**