# The ZipArchive class





Zip a folder (include itself).

Usage:

&#xA0; HZip::zipDir(&apos;/path/to/sourceDir&apos;, &apos;/path/to/out.zip&apos;);





```
<?php

class HZip

{

&#xA0; /**

&#xA0;&#xA0; * Add files and sub-directories in a folder to zip file.

&#xA0;&#xA0; * @param string $folder

&#xA0;&#xA0; * @param ZipArchive $zipFile

&#xA0;&#xA0; * @param int $exclusiveLength Number of text to be exclusived from the file path.

&#xA0;&#xA0; */

&#xA0; private static function folderToZip($folder, &amp;$zipFile, $exclusiveLength) {

&#xA0; &#xA0; $handle = opendir($folder);

&#xA0; &#xA0; while (false !== $f = readdir($handle)) {

&#xA0; &#xA0; &#xA0; if ($f != &apos;.&apos; &amp;&amp; $f != &apos;..&apos;) {

&#xA0; &#xA0; &#xA0; &#xA0; $filePath = &quot;$folder/$f&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; // Remove prefix from file path before add to zip.

&#xA0; &#xA0; &#xA0; &#xA0; $localPath = substr($filePath, $exclusiveLength);

&#xA0; &#xA0; &#xA0; &#xA0; if (is_file($filePath)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $zipFile-&gt;addFile($filePath, $localPath);

&#xA0; &#xA0; &#xA0; &#xA0; } elseif (is_dir($filePath)) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Add sub-directory.

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $zipFile-&gt;addEmptyDir($localPath);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::folderToZip($filePath, $zipFile, $exclusiveLength);

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; closedir($handle);

&#xA0; }



&#xA0; /**

&#xA0;&#xA0; * Zip a folder (include itself).

&#xA0;&#xA0; * Usage:

&#xA0;&#xA0; *&#xA0;&#xA0; HZip::zipDir(&apos;/path/to/sourceDir&apos;, &apos;/path/to/out.zip&apos;);

&#xA0;&#xA0; *

&#xA0;&#xA0; * @param string $sourcePath Path of directory to be zip.

&#xA0;&#xA0; * @param string $outZipPath Path of output zip file.

&#xA0;&#xA0; */

&#xA0; public static function zipDir($sourcePath, $outZipPath)

&#xA0; {

&#xA0; &#xA0; $pathInfo = pathInfo($sourcePath);

&#xA0; &#xA0; $parentPath = $pathInfo[&apos;dirname&apos;];

&#xA0; &#xA0; $dirName = $pathInfo[&apos;basename&apos;];



&#xA0; &#xA0; $z = new ZipArchive();

&#xA0; &#xA0; $z-&gt;open($outZipPath, ZIPARCHIVE::CREATE);

&#xA0; &#xA0; $z-&gt;addEmptyDir($dirName);

&#xA0; &#xA0; self::folderToZip($sourcePath, $z, strlen(&quot;$parentPath/&quot;));

&#xA0; &#xA0; $z-&gt;close();

&#xA0; }

}

?>
```



  

#



With PHP 5.6+, you may come up with theses errors.

Warning: Unknown: Cannot destroy the zip context in Unknown on line 0

Warning: ZipArchive::close(): Can&apos;t remove file: No such file or directory in xxxx.php on line xx

Examples

Warning: Unknown: Cannot destroy the zip context in Unknown on line 0



```
<?php&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; $za = new ZipArchive;
&#xA0; &#xA0; $za-&gt;open(&apos;51-n.com.zip&apos;,ZipArchive::CREATE|ZipArchive::OVERWRITE);
?>
```


Warning: ZipArchive::close(): Can&apos;t remove file: No such file or directory in xxxx.php on line xx



```
<?php&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; $za = new ZipArchive;
&#xA0; &#xA0; $za-&gt;open(&apos;51-n.com.zip&apos;,ZipArchive::CREATE|ZipArchive::OVERWRITE);
&#xA0; &#xA0; $za-&gt;close();
?>
```


It happens when the zip archive is empty.
Your zip archive will not be saved on disk unless it has at least one file. What&apos;s more, when ZipArchive::OVERWRITE is applied, if there exists a file with the same name, it will be removed after ZipArchive::open() is called.

So, don&apos;t forget to put at least one file to your zip archive.



```
<?php&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; $za = new ZipArchive;
&#xA0; &#xA0; $za-&gt;open(&apos;51-n.com.zip&apos;,ZipArchive::CREATE|ZipArchive::OVERWRITE);
&#xA0; &#xA0; $za-&gt;addFromString(&apos;wuxiancheng.cn.txt&apos;,&apos;yes&apos;);
&#xA0; &#xA0; $za-&gt;close();
?>
```



  

#



There is a usefull function to get the ZipArchive status as a human readable string :



```
<?php
function ZipStatusString( $status )
{
&#xA0; &#xA0; switch( (int) $status )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_OK&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : return &apos;N No error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_MULTIDISK&#xA0; &#xA0; : return &apos;N Multi-disk zip archives not supported&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_RENAME&#xA0; &#xA0; &#xA0;&#xA0; : return &apos;S Renaming temporary file failed&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_CLOSE&#xA0; &#xA0; &#xA0; &#xA0; : return &apos;S Closing zip archive failed&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_SEEK&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : return &apos;S Seek error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_READ&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : return &apos;S Read error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_WRITE&#xA0; &#xA0; &#xA0; &#xA0; : return &apos;S Write error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_CRC&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : return &apos;N CRC error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_ZIPCLOSED&#xA0; &#xA0; : return &apos;N Containing zip archive was closed&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_NOENT&#xA0; &#xA0; &#xA0; &#xA0; : return &apos;N No such file&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_EXISTS&#xA0; &#xA0; &#xA0;&#xA0; : return &apos;N File already exists&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_OPEN&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : return &apos;S Can\&apos;t open file&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_TMPOPEN&#xA0; &#xA0; &#xA0; : return &apos;S Failure to create temporary file&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_ZLIB&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : return &apos;Z Zlib error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_MEMORY&#xA0; &#xA0; &#xA0;&#xA0; : return &apos;N Malloc failure&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_CHANGED&#xA0; &#xA0; &#xA0; : return &apos;N Entry has been changed&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_COMPNOTSUPP&#xA0; : return &apos;N Compression method not supported&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_EOF&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; : return &apos;N Premature EOF&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_INVAL&#xA0; &#xA0; &#xA0; &#xA0; : return &apos;N Invalid argument&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_NOZIP&#xA0; &#xA0; &#xA0; &#xA0; : return &apos;N Not a zip archive&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_INTERNAL&#xA0; &#xA0;&#xA0; : return &apos;N Internal error&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_INCONS&#xA0; &#xA0; &#xA0;&#xA0; : return &apos;N Zip archive inconsistent&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_REMOVE&#xA0; &#xA0; &#xA0;&#xA0; : return &apos;S Can\&apos;t remove file&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; case ZipArchive::ER_DELETED&#xA0; &#xA0; &#xA0; : return &apos;N Entry has been deleted&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; default: return sprintf(&apos;Unknown status %s&apos;, $status );
&#xA0; &#xA0; }
}

?>
```



  

#



A way of zipping files and downloading them thereafter:


```
<?php

 $files = array(&apos;image.jpeg&apos;,&apos;text.txt&apos;,&apos;music.wav&apos;);
$zipname = &apos;enter_any_name_for_the_zipped_file.zip&apos;;
$zip = new ZipArchive;
$zip-&gt;open($zipname, ZipArchive::CREATE);
foreach ($files as $file) {
&#xA0; $zip-&gt;addFile($file);
}
$zip-&gt;close();

///Then download the zipped file.
header(&apos;Content-Type: application/zip&apos;);
header(&apos;Content-disposition: attachment; filename=&apos;.$zipname);
header(&apos;Content-Length: &apos; . filesize($zipname));
readfile($zipname);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.ziparchive.php)

**[To root](/README.md)**