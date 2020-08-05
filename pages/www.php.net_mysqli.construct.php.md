# mysqli::__construct



Please do use set_charset("utf8") after establishing the connection if you want to avoid weird string issues. I do not know why the documentation does not warn you about this kind of stuff.<br><br>We had a hard time figuring out what was going on since we were using mb_detect_encoding and it said everything was UTF-8, but of course the display was wrong. If we used iconv from ISO-8859-1 to UTF-8 the strings looked fine, even though everything in the database had the right collation. So in the end, it was the connection that was the filter and although the notes for this function mention default charsets, it almost reads as a sidenote instead of a central issue when dealing with UTF and PHP/MySQL.  

---

Note that on all &gt;=Windows 7 Servers, a host name "localhost" will create a very expensive lookup (~1 Second). <br><br>That&apos;s because since Windows 7, the hosts file doesn&apos;t come with a preconfigured<br>127.0.0.1 localhost<br>anymore<br><br>So, if you notice a long connection creation, try "127.0.0.1" instead.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli.construct.php)

**[To root](/README.md)**