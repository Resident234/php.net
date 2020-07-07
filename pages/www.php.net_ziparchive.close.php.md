# ZipArchive::close





It may seem a little obvious to some but it was an oversight on my behalf.

If you are adding files to the zip file that you want to be deleted make sure you delete AFTER you call the close() function.

If the files added to the object aren&apos;t available at save time the zip file will not be created.

  

#



If you&apos;re adding multiple files to a zip and your $zip-&gt;close() call is returning FALSE, ensure that all the files you added actually exist. Apparently $zip-&gt;addFile() returns TRUE even if the file doesn&apos;t actually exist. It&apos;s a good idea to check each file with file_exists() or is_readable() before calling $zip-&gt;addFile() on it.

  

#



ZipArchive.close() changes its behaviour in PHP7. The function ignores directories in PHP5 but fails in PHP7 with: 

Unexpected PHP error [ZipArchive::close(): Read error: Is a directory]

The following code works in PHP5 but not in PHP7:



```
<?php 
// test.php
$zip = new ZipArchive();
$zip-&gt;open(&apos;/tmp/test.zip&apos;, ZipArchive::CREATE);
$zip-&gt;addFile(&apos;.&apos;, &apos;.&apos;);
$ret = $zip-&gt;close();
echo &quot;Closed with: &quot; . ($ret ? &quot;true&quot; : &quot;false&quot;) . &quot;\n&quot;;
?>
```


For php5:

php --version
&#xA0; PHP 5.5.38-1-avature-ondrej-fork (cli) (built: Aug 31 2016 16:37:38)

php test.php
&#xA0; Closed with: true

For php7:

php --version
&#xA0; PHP 7.0.8-0ubuntu0.16.04.2 (cli) ( NTS )

php test.php
&#xA0; Closed with: false

  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.close.php)

**[To root](/README.md)**