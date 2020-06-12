# The character set and character escaping




<div class="phpcode"><span class="html">
Please note that MySQL&apos;s utf8 encoding has a maximum of 3 bytes and is unable to encode *all* unicode characters.<br><br>If you need to encode characters beyond the BMP (Basic Multilingual Plane), like emoji or other special characters, you will need to use a different encoding like utf8mb4 or any other encoding supporting the higher planes. Mysql will discard any characters encoded in 4 bytes (or more).<br><br>See <a href="https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-utf8mb4.html" rel="nofollow" target="_blank">https://dev.mysql.com/doc/refman/5.7/en/charset-unicode-utf8mb4.html</a> for more information on the matter</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqlinfo.concepts.charset.php)

**[⬆ to root](/)**