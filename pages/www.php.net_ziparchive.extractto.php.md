# ZipArchive::extractTo



If you want to copy one file at a time and remove the folder name that is stored in the ZIP file, so you don&apos;t have to create directories from the ZIP itself, then use this snippet (basically collapses the ZIP file into one Folder).<br><br>

```
<?php

$path = 'zipfile.zip'

$zip = new ZipArchive;
if ($zip->open($path) === true) {
    for($i = 0; $i &lt; $zip->numFiles; $i++) {
        $filename = $zip->getNameIndex($i);
        $fileinfo = pathinfo($filename);
        copy("zip://".$path."#".$filename, "/your/new/destination/".$fileinfo['basename']);
    }                   
    $zip->close();                   
}

?>
```
<br><br>* On a side note, you can also use $_FILES[&apos;userfile&apos;][&apos;tmp_name&apos;] as the $path for an uploaded ZIP so you never have to move it or extract a uploaded zip file.<br><br>Cheers!<br><br>ProNeticas Dev Team  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.extractto.php)

**[To root](/README.md)**