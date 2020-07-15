# ZipArchive::setPassword



It seems that this function supports only decryption of password protected archives (see changelog: http://pecl.php.net/package-changelog.php?package=zip). Creation of password protected archives is not supported (they will be created simply as non-protected archives).<br><br>Example code for extraction of files from password protected ZIP archives:<br><br>

```
<?php
    $zip = new ZipArchive();
    $zip_status = $zip->open("test.zip");

    if ($zip_status === true)
    {
        if ($zip->setPassword("MySecretPassword"))
        {
            if (!$zip->extractTo(__DIR__))
                echo "Extraction failed (wrong password?)";
        }

        $zip->close();
    }
    else
    {
        die("Failed opening archive: ". @$zip->getStatusString() . " (code: ". $zip_status .")");
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.setpassword.php)

**[To root](/README.md)**