# finfo_open



I am running Windows 7 with Apache.  It took hours to figure out why it was not working.<br><br>First, enable the php_fileinfo.dll extension in you php.ini. You&apos;ll also need the four magic files that are found in the following library:<br><br>http://sourceforge.net/projects/gnuwin32/files/file/4.23/file-4.23-bin.zip/download<br><br>An environment variable or a direct path to the file named "magic" is necessary, without any extension.  <br><br>Then, make sure that xdebug is either turned off or set the ini error_reporting to not display notices or warnings for the script.<br><br>Hope this saves someone a few hours of frustration!  

#

For most common image files:<br>

```
<?php
function minimime($fname) {
    $fh=fopen($fname,&apos;rb&apos;);
    if ($fh) { 
        $bytes6=fread($fh,6);
        fclose($fh); 
        if ($bytes6===false) return false;
        if (substr($bytes6,0,3)=="\xff\xd8\xff") return &apos;image/jpeg&apos;;
        if ($bytes6=="\x89PNG\x0d\x0a") return &apos;image/png&apos;;
        if ($bytes6=="GIF87a" || $bytes6=="GIF89a") return &apos;image/gif&apos;;
        return &apos;application/octet-stream&apos;;
    }
    return false;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-open.php)

**[To root](/README.md)**