# symlink



Here is a simple way to control who downloads your files...<br><br>You will have to set: $filename, $downloaddir, $safedir and $downloadURL.<br><br>Basically $filename is the name of a file, $downloaddir is any dir on your server, $safedir is a dir that is not accessible by a browser that contains a file named $filename and $downloadURL is the URL equivalent of your $downloaddir.<br><br>The way this works is when a user wants to download a file, a randomly named dir is created in the $downloaddir, and a symbolic link is created to the file being requested.  The browser is then redirected to the new link and the download begins.<br><br>The code also deletes any past symbolic links created by any past users before creating one for itself.  This in effect leaves only one symbolic link at a time and prevents past users from downloading the file again without going through this script.  There appears to be no problem if a symbolic link is deleted while another person is downloading from that link.<br><br>This is not too great if not many people download the file since the symbolic link will not be deleted until another person downloads the same file. <br><br>Anyway enjoy:<br><br>

```
<?php
$letters = 'abcdefghijklmnopqrstuvwxyz';
srand((double) microtime() * 1000000);
$string = '';
for ($i = 1; $i <= rand(4,12); $i++) {
   $q = rand(1,24);
   $string = $string . $letters[$q];
}
$handle = opendir($downloaddir);
while ($dir = readdir($handle)) {
   if (is_dir($downloaddir . $dir)){
      if ($dir != "." &amp;&amp; $dir != ".."){
         @unlink($downloaddir . $dir . "/" . $filename);
         @rmdir($downloaddir . $dir);
      }
   }
}
closedir($handle);
mkdir($downloaddir . $string, 0777);
symlink($safedir . $filename, $downloaddir . $string . "/" . $filename);
Header("Location: " . $downloadURL . $string . "/" . $filename);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.symlink.php)

**[To root](/README.md)**