# ZipArchive::extractTo





If you want to copy one file at a time and remove the folder name that is stored in the ZIP file, so you don&apos;t have to create directories from the ZIP itself, then use this snippet (basically collapses the ZIP file into one Folder).



```
<?php

$path = &apos;zipfile.zip&apos;

$zip = new ZipArchive;
if ($zip-&gt;open($path) === true) {
&#xA0; &#xA0; for($i = 0; $i &lt; $zip-&gt;numFiles; $i++) {
&#xA0; &#xA0; &#xA0; &#xA0; $filename = $zip-&gt;getNameIndex($i);
&#xA0; &#xA0; &#xA0; &#xA0; $fileinfo = pathinfo($filename);
&#xA0; &#xA0; &#xA0; &#xA0; copy(&quot;zip://&quot;.$path.&quot;#&quot;.$filename, &quot;/your/new/destination/&quot;.$fileinfo[&apos;basename&apos;]);
&#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; $zip-&gt;close();&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
}

?>
```


* On a side note, you can also use $_FILES[&apos;userfile&apos;][&apos;tmp_name&apos;] as the $path for an uploaded ZIP so you never have to move it or extract a uploaded zip file.

Cheers!

ProNeticas Dev Team

  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.extractto.php)

**[To root](/README.md)**