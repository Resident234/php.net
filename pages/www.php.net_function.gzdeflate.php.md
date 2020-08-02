# gzdeflate



Take care that that "PHP deflate" != "HTTP deflate".<br><br>The deflate encoding used in HTTP is actually zlib encoded.<br><br>This is what PHP functions return:<br>gzencode() == gzip<br>gzcompress() == zlib (aka. HTTP deflate)<br>gzdeflate()  == *raw* deflate encoding  

---

[Official documentation page](https://www.php.net/manual/en/function.gzdeflate.php)

**[To root](/README.md)**