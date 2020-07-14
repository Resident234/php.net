# ZipArchive::addFile



It is not obvious, since there are no noticeable examples around, but you can use $localname (second parameter) to define and control file/directory structure inside the zip. Use it if you do not want files to be included with their absolute directory tree.<br><br>

```
<?php

$zip-&gt;addFile($abs_path, $relative_path);

?>
```
  

#

Beware: calling $zip-&gt;addFile() on a file that doesn&apos;t exist will succeed and return TRUE, delaying the failure until you make the final $zip-&gt;close() call, which will return FALSE and potentially leave you scratching your head.<br><br>If you&apos;re adding multiple files to a zip and your $zip-&gt;close() call is returning FALSE, ensure that all the files you added actually exist.<br><br>It&apos;s also a good idea to check each file with file_exists() or is_readable() before calling $zip-&gt;addFile() on it.  

#

When adding a file to your zip, the file is opened and stays open.<br>When adding over 1024 files (depending on your open files limit) the server stops adding files, resulting in a status 11 in your zip Archive. There is no warning when exceeding this open files limit with addFiles.<br><br>Check your open files with ulimit -a<br><br>This kept me busy for some time.  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.addfile.php)

**[To root](/README.md)**