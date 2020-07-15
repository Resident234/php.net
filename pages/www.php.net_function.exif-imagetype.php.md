# exif_imagetype



Because I only want to check for jpeg or png from a memory string, this is my 2 functions that are quick and don&apos;t have any dependencies :<br><br>

```
<?php
  function is_jpeg(&amp;$pict)
  {
    return (bin2hex($pict[0]) == 'ff' &amp;&amp; bin2hex($pict[1]) == 'd8');
  }

  function is_png(&amp;$pict)
  {
    return (bin2hex($pict[0]) == '89' &amp;&amp; $pict[1] == 'P' &amp;&amp; $pict[2] == 'N' &amp;&amp; $pict[3] == 'G');
  }
?>
```
  

#

By trial and error, it seems that a file has to be 12 bytes or larger in order to avoid a "Read error!".  Here&apos;s a work-around to avoid an error being thrown:<br><br>// exif_imagetype throws "Read error!" if file is too small<br>if (filesize($uploadfile) &gt; 11)<br>    $mimetype = exif_imagetype($uploadfile);<br>else<br>    $mimetype = false;  

#

Windows users: If you get the fatal error "Fatal error:  Call to undefined function exif_imagetype()", and you have enabled php_exif.dll, make sure you enable php_mbstring.dll. You must put mbstring before exif in the php.ini, i.e.:<br><br>extension=php_mbstring.dll<br>extension=php_exif.dll<br><br>You can check whether this has worked by calling phpinfo() and searching for exif.  

#

[Official documentation page](https://www.php.net/manual/en/function.exif-imagetype.php)

**[To root](/README.md)**