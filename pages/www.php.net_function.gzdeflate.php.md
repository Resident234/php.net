# gzdeflate





Take care that that &quot;PHP deflate&quot; != &quot;HTTP deflate&quot;.

The deflate encoding used in HTTP is actually zlib encoded.

This is what PHP functions return:
gzencode() == gzip
gzcompress() == zlib (aka. HTTP deflate)
gzdeflate()&#xA0; == *raw* deflate encoding

  

#

[Official documentation page](https://www.php.net/manual/en/function.gzdeflate.php)

**[To root](/README.md)**