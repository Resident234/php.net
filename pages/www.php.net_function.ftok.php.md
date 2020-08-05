# ftok



Thanks to  daniele_dll@yahoo.it who got this in turn from linux glibc 2.3.2: http://www.php.net/manual/en/function.shmop-open.php -- I&apos;m putting this here because it might be helpful to others.<br><br>function ftok($pathname, $proj_id) {<br>   $st = @stat($pathname);<br>   if (!$st) {<br>       return -1;<br>   }<br>  <br>   $key = sprintf("%u", (($st[&apos;ino&apos;] &amp; 0xffff) | (($st[&apos;dev&apos;] &amp; 0xff) &lt;&lt; 16) | (($proj_id &amp; 0xff) &lt;&lt; 24)));<br>   return $key;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.ftok.php)

**[To root](/README.md)**