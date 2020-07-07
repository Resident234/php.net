# finfo_buffer





You can easily check mime type of an internet resource using this code :



```
<?php
function getUrlMimeType($url) {
&#xA0; &#xA0; $buffer = file_get_contents($url);
&#xA0; &#xA0; $finfo = new finfo(FILEINFO_MIME_TYPE);
&#xA0; &#xA0; return $finfo-&gt;buffer($buffer);
}
?>
```


I&apos;m using it to detect if an url given by a user is a HTML page (so I do some stuff with the HTML) or a file on Internet (so I show an icon accordingly to the mime type).

  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-buffer.php)

**[To root](/README.md)**