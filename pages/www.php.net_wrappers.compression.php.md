# zlib://



One-liners to gzip and ungzip a file:<br><br>copy(&apos;file.txt&apos;, &apos;compress.zlib://&apos; . &apos;file.txt.gz&apos;);<br><br>copy(&apos;compress.zlib://&apos; . &apos;file.txt.gz&apos;, &apos;file.txt&apos;);  

---

Example on how to read an entry from a ZIP archive (file "bar.txt" inside "./foo.zip"):<br><br>

```
<?php

$fp = fopen('zip://./foo.zip#bar.txt', 'r');
if( $fp ){
    while( !feof($fp) ){
        echo fread($fp, 8192);
    }
    fclose($fp);
}

?>
```
<br><br>Also, apparently, the "zip:" wrapper does not allow writing as of PHP/5.3.6. You can read http://php.net/ziparchive-getstream for further reference since the underlying code is probably the same.  

---

[Official documentation page](https://www.php.net/manual/en/wrappers.compression.php)

**[To root](/README.md)**