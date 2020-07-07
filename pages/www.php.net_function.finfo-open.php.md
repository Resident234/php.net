# finfo_open





I am running Windows 7 with Apache.&#xA0; It took hours to figure out why it was not working.

First, enable the php_fileinfo.dll extension in you php.ini. You&apos;ll also need the four magic files that are found in the following library:

http://sourceforge.net/projects/gnuwin32/files/file/4.23/file-4.23-bin.zip/download

An environment variable or a direct path to the file named &quot;magic&quot; is necessary, without any extension.&#xA0; 

Then, make sure that xdebug is either turned off or set the ini error_reporting to not display notices or warnings for the script.

Hope this saves someone a few hours of frustration!

  

#



For most common image files:


```
<?php
function minimime($fname) {
&#xA0; &#xA0; $fh=fopen($fname,&apos;rb&apos;);
&#xA0; &#xA0; if ($fh) { 
&#xA0; &#xA0; &#xA0; &#xA0; $bytes6=fread($fh,6);
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fh); 
&#xA0; &#xA0; &#xA0; &#xA0; if ($bytes6===false) return false;
&#xA0; &#xA0; &#xA0; &#xA0; if (substr($bytes6,0,3)==&quot;\xff\xd8\xff&quot;) return &apos;image/jpeg&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; if ($bytes6==&quot;\x89PNG\x0d\x0a&quot;) return &apos;image/png&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; if ($bytes6==&quot;GIF87a&quot; || $bytes6==&quot;GIF89a&quot;) return &apos;image/gif&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;application/octet-stream&apos;;
&#xA0; &#xA0; }
&#xA0; &#xA0; return false;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-open.php)

**[To root](/README.md)**