# ftp_mkdir



Here&apos;s the correct code for making recursive directories:<br><br>

```
<?php

// function
function ftp_mksubdirs($ftpcon,$ftpbasedir,$ftpath){
   @ftp_chdir($ftpcon, $ftpbasedir); // /var/www/uploads
   $parts = explode('/',$ftpath); // 2013/06/11/username
   foreach($parts as $part){
      if(!@ftp_chdir($ftpcon, $part)){
         ftp_mkdir($ftpcon, $part);
         ftp_chdir($ftpcon, $part);
         //ftp_chmod($ftpcon, 0777, $part);
      }
   }
}

// usage
$path_of_storage = '/var/www/uploads';
$newftpdir = '2013/06/11/username';

$conn_id = ftp_connect($ftpserver);
ftp_login($conn_id, $login, $pass);
ftp_mksubdirs($conn_id,$path_of_storage,$newftpdir);
ftp_close($conn_id);

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.ftp-mkdir.php)

**[To root](/README.md)**