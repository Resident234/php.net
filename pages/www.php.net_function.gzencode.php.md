# gzencode



Had some trouble finding the correct way to send a Content-Length header with HTTP compression.<br>The pitch is to use gzencode (not gzdeflaten not gzcompress).<br><br>

```
<?php

// disable ZLIB ouput compression
ini_set('zlib.output_compression','Off');

// compress data
$gzipoutput = gzencode($output,6);

// various headers, those with # are mandatory
header('Content-Type: application/x-download');
header('Content-Encoding: gzip'); #
header('Content-Length: '.strlen($gzipoutput)); #
header('Content-Disposition: attachment; filename="myfile.name"');
header('Cache-Control: no-cache, no-store, max-age=0, must-revalidate');
header('Pragma: no-cache');

// output data
echo $gzipoutput;

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.gzencode.php)

**[To root](/README.md)**