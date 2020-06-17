# gzdeflate




<div class="phpcode"><span class="html">
Take care that that &quot;PHP deflate&quot; != &quot;HTTP deflate&quot;.<br><br>The deflate encoding used in HTTP is actually zlib encoded.<br><br>This is what PHP functions return:<br>gzencode() == gzip<br>gzcompress() == zlib (aka. HTTP deflate)<br>gzdeflate()&#xA0; == *raw* deflate encoding</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gzdeflate.php)

**[To root](/README.md)**