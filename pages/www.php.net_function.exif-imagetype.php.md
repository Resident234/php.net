# exif_imagetype





Because I only want to check for jpeg or png from a memory string, this is my 2 functions that are quick and don&apos;t have any dependencies :



```
<?php
&#xA0; function is_jpeg(&amp;$pict)
&#xA0; {
&#xA0; &#xA0; return (bin2hex($pict[0]) == &apos;ff&apos; &amp;&amp; bin2hex($pict[1]) == &apos;d8&apos;);
&#xA0; }

&#xA0; function is_png(&amp;$pict)
&#xA0; {
&#xA0; &#xA0; return (bin2hex($pict[0]) == &apos;89&apos; &amp;&amp; $pict[1] == &apos;P&apos; &amp;&amp; $pict[2] == &apos;N&apos; &amp;&amp; $pict[3] == &apos;G&apos;);
&#xA0; }
?>
```



  

#



By trial and error, it seems that a file has to be 12 bytes or larger in order to avoid a &quot;Read error!&quot;.&#xA0; Here&apos;s a work-around to avoid an error being thrown:

// exif_imagetype throws &quot;Read error!&quot; if file is too small
if (filesize($uploadfile) &gt; 11)
&#xA0; &#xA0; $mimetype = exif_imagetype($uploadfile);
else
&#xA0; &#xA0; $mimetype = false;

  

#



Windows users: If you get the fatal error &quot;Fatal error:&#xA0; Call to undefined function exif_imagetype()&quot;, and you have enabled php_exif.dll, make sure you enable php_mbstring.dll. You must put mbstring before exif in the php.ini, i.e.:

extension=php_mbstring.dll
extension=php_exif.dll

You can check whether this has worked by calling phpinfo() and searching for exif.

  

#

[Official documentation page](https://www.php.net/manual/en/function.exif-imagetype.php)

**[To root](/README.md)**