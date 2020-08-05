# fopen



Note - using fopen in &apos;w&apos; mode will NOT update the modification time (filemtime) of a file like you may expect. You may want to issue a touch() after writing and closing the file which update its modification time. This may become critical in a caching situation, if you intend to keep your hair.  

---

With php 5.2.5 on Apache 2.2.4, accessing files on an ftp server with fopen() or readfile() requires an extra forwardslash if an absolute path is needed.<br><br>i.e., if a file called bullbes.txt is stored under /var/school/ on ftp server example.com and you&apos;re trying to access it with user blossom and password buttercup, the url would be:<br><br>ftp://blossom:buttercup@example.com//var/school/bubbles.txt<br><br>Note the two forwardslashes. It looks like the second one is needed so the server won&apos;t interpret the path as relative to blossom&apos;s home on townsville.  

---

[Official documentation page](https://www.php.net/manual/en/function.fopen.php)

**[To root](/README.md)**