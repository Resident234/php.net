# ZipArchive::setPassword



It seems that this function supports only decryption of password protected archives (see changelog: http://pecl.php.net/package-changelog.php?package=zip). Creation of password protected archives is not supported (they will be created simply as non-protected archives).<br><br>Example code for extraction of files from password protected ZIP archives:<br><br>

```
<?php
    $zip = new ZipArchive();
    $zip_status = $zip-&gt;open("test.zip");

    if ($zip_status === true)
    {
        if ($zip-&gt;setPassword("MySecretPassword"))
        {
            if (!$zip-&gt;extractTo(__DIR__))
                echo "Extraction failed (wrong password?)";
        }

        $zip-&gt;close();
    }
    else
    {
        die("Failed opening archive: ". @$zip-&gt;getStatusString() . " (code: ". $zip_status .")");
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.setpassword.php)

**[To root](/README.md)**