# gzencode





Had some trouble finding the correct way to send a Content-Length header with HTTP compression.
The pitch is to use gzencode (not gzdeflaten not gzcompress).



```
<?php

// disable ZLIB ouput compression
ini_set(&apos;zlib.output_compression&apos;,&apos;Off&apos;);

// compress data
$gzipoutput = gzencode($output,6);

// various headers, those with # are mandatory
header(&apos;Content-Type: application/x-download&apos;);
header(&apos;Content-Encoding: gzip&apos;); #
header(&apos;Content-Length: &apos;.strlen($gzipoutput)); #
header(&apos;Content-Disposition: attachment; filename=&quot;myfile.name&quot;&apos;);
header(&apos;Cache-Control: no-cache, no-store, max-age=0, must-revalidate&apos;);
header(&apos;Pragma: no-cache&apos;);

// output data
echo $gzipoutput;

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.gzencode.php)

**[To root](/README.md)**