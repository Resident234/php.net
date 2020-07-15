# tmpfile



To get the underlying file path of a tmpfile file pointer:<br><br>

```
<?php
$file = tmpfile();
$path = stream_get_meta_data($file)['uri']; // eg: /tmp/phpFx0513a?>
```
  

#

I found this function useful when uploading a file through FTP. One of the files I was uploading was input from a textarea on the previous page, so really there was no "file" to upload, this solved the problem nicely:<br><br>

```
<?php
    # Upload setup.inc
    $fSetup = tmpfile();
    fwrite($fSetup,$setup);
    fseek($fSetup,0);
    if (!ftp_fput($ftp,"inc/setup.inc",$fSetup,FTP_ASCII)) {
        echo "&lt;br /&gt;&lt;i&gt;Setup file NOT inserted&lt;/i&gt;&lt;br /&gt;&lt;br /&gt;";
    }
    fclose($fSetup);
?>
```
<br><br>The $setup variable is the contents of the textarea.<br><br>And I&apos;m not sure if you need the fseek($temp,0); in there either, just leave it unless you know it doesn&apos;t effect it.  

#

[Official documentation page](https://www.php.net/manual/en/function.tmpfile.php)

**[To root](/README.md)**