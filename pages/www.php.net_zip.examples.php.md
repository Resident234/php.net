# Examples



All these examples will not work if the php script has no write access within the folder. <br><br>Although you may say this is obvious, I found that in this case, $zip-&gt;open("name", ZIPARCHIVE::CREATE) doesn&apos;t return an error as it might not try to access the file system but rather allocates memory. <br><br>It is only $zip-&gt;close() that returns the error. This might cause you seeking at the wrong end.  

#



```
<?php
        $zip = new ZipArchive;
    $zip->open('teste.zip');
    $zip->extractTo('./');
    $zip->close();
        echo "Ok!";
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/zip.examples.php)

**[To root](/README.md)**