# ftp_fput



For directly inserting content into a file on an FTP host, you could also create a string stream wich streams directly to the ftp_fput function. <br><br>This should create less overhead than first writing to any temp directories locally before streaming, as suggested here.<br><br>

```
<?php

$string = "Your content goes here";
$stream = fopen('data://text/plain,' . $string,'r');

ftp_fput($this->connection,$pathTo,$stream, FTP_BINARY);

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.ftp-fput.php)

**[To root](/README.md)**