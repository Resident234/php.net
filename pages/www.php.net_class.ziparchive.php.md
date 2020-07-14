# The ZipArchive class



Zip a folder (include itself).<br>Usage:<br>  HZip::zipDir(&apos;/path/to/sourceDir&apos;, &apos;/path/to/out.zip&apos;);<br><br>

```
<?php
class HZip
{
  /**
   * Add files and sub-directories in a folder to zip file.
   * @param string $folder
   * @param ZipArchive $zipFile
   * @param int $exclusiveLength Number of text to be exclusived from the file path.
   */
  private static function folderToZip($folder, &amp;$zipFile, $exclusiveLength) {
    $handle = opendir($folder);
    while (false !== $f = readdir($handle)) {
      if ($f != &apos;.&apos; &amp;&amp; $f != &apos;..&apos;) {
        $filePath = "$folder/$f";
        // Remove prefix from file path before add to zip.
        $localPath = substr($filePath, $exclusiveLength);
        if (is_file($filePath)) {
          $zipFile-&gt;addFile($filePath, $localPath);
        } elseif (is_dir($filePath)) {
          // Add sub-directory.
          $zipFile-&gt;addEmptyDir($localPath);
          self::folderToZip($filePath, $zipFile, $exclusiveLength);
        }
      }
    }
    closedir($handle);
  }

  /**
   * Zip a folder (include itself).
   * Usage:
   *   HZip::zipDir(&apos;/path/to/sourceDir&apos;, &apos;/path/to/out.zip&apos;);
   *
   * @param string $sourcePath Path of directory to be zip.
   * @param string $outZipPath Path of output zip file.
   */
  public static function zipDir($sourcePath, $outZipPath)
  {
    $pathInfo = pathInfo($sourcePath);
    $parentPath = $pathInfo[&apos;dirname&apos;];
    $dirName = $pathInfo[&apos;basename&apos;];

    $z = new ZipArchive();
    $z-&gt;open($outZipPath, ZIPARCHIVE::CREATE);
    $z-&gt;addEmptyDir($dirName);
    self::folderToZip($sourcePath, $z, strlen("$parentPath/"));
    $z-&gt;close();
  }
}
?>
```
  

#

With PHP 5.6+, you may come up with theses errors.<br><br>Warning: Unknown: Cannot destroy the zip context in Unknown on line 0<br><br>Warning: ZipArchive::close(): Can&apos;t remove file: No such file or directory in xxxx.php on line xx<br><br>Examples<br><br>Warning: Unknown: Cannot destroy the zip context in Unknown on line 0<br><br>

```
<?php         
    $za = new ZipArchive;
    $za-&gt;open(&apos;51-n.com.zip&apos;,ZipArchive::CREATE|ZipArchive::OVERWRITE);
?>
```


Warning: ZipArchive::close(): Can&apos;t remove file: No such file or directory in xxxx.php on line xx



```
<?php         
    $za = new ZipArchive;
    $za-&gt;open(&apos;51-n.com.zip&apos;,ZipArchive::CREATE|ZipArchive::OVERWRITE);
    $za-&gt;close();
?>
```


It happens when the zip archive is empty.
Your zip archive will not be saved on disk unless it has at least one file. What&apos;s more, when ZipArchive::OVERWRITE is applied, if there exists a file with the same name, it will be removed after ZipArchive::open() is called.

So, don&apos;t forget to put at least one file to your zip archive.



```
<?php         
    $za = new ZipArchive;
    $za-&gt;open(&apos;51-n.com.zip&apos;,ZipArchive::CREATE|ZipArchive::OVERWRITE);
    $za-&gt;addFromString(&apos;wuxiancheng.cn.txt&apos;,&apos;yes&apos;);
    $za-&gt;close();
?>
```
  

#

There is a usefull function to get the ZipArchive status as a human readable string :<br><br>

```
<?php
function ZipStatusString( $status )
{
    switch( (int) $status )
    {
        case ZipArchive::ER_OK           : return &apos;N No error&apos;;
        case ZipArchive::ER_MULTIDISK    : return &apos;N Multi-disk zip archives not supported&apos;;
        case ZipArchive::ER_RENAME       : return &apos;S Renaming temporary file failed&apos;;
        case ZipArchive::ER_CLOSE        : return &apos;S Closing zip archive failed&apos;;
        case ZipArchive::ER_SEEK         : return &apos;S Seek error&apos;;
        case ZipArchive::ER_READ         : return &apos;S Read error&apos;;
        case ZipArchive::ER_WRITE        : return &apos;S Write error&apos;;
        case ZipArchive::ER_CRC          : return &apos;N CRC error&apos;;
        case ZipArchive::ER_ZIPCLOSED    : return &apos;N Containing zip archive was closed&apos;;
        case ZipArchive::ER_NOENT        : return &apos;N No such file&apos;;
        case ZipArchive::ER_EXISTS       : return &apos;N File already exists&apos;;
        case ZipArchive::ER_OPEN         : return &apos;S Can\&apos;t open file&apos;;
        case ZipArchive::ER_TMPOPEN      : return &apos;S Failure to create temporary file&apos;;
        case ZipArchive::ER_ZLIB         : return &apos;Z Zlib error&apos;;
        case ZipArchive::ER_MEMORY       : return &apos;N Malloc failure&apos;;
        case ZipArchive::ER_CHANGED      : return &apos;N Entry has been changed&apos;;
        case ZipArchive::ER_COMPNOTSUPP  : return &apos;N Compression method not supported&apos;;
        case ZipArchive::ER_EOF          : return &apos;N Premature EOF&apos;;
        case ZipArchive::ER_INVAL        : return &apos;N Invalid argument&apos;;
        case ZipArchive::ER_NOZIP        : return &apos;N Not a zip archive&apos;;
        case ZipArchive::ER_INTERNAL     : return &apos;N Internal error&apos;;
        case ZipArchive::ER_INCONS       : return &apos;N Zip archive inconsistent&apos;;
        case ZipArchive::ER_REMOVE       : return &apos;S Can\&apos;t remove file&apos;;
        case ZipArchive::ER_DELETED      : return &apos;N Entry has been deleted&apos;;
        
        default: return sprintf(&apos;Unknown status %s&apos;, $status );
    }
}

?>
```
  

#

A way of zipping files and downloading them thereafter:<br>

```
<?php

 $files = array(&apos;image.jpeg&apos;,&apos;text.txt&apos;,&apos;music.wav&apos;);
$zipname = &apos;enter_any_name_for_the_zipped_file.zip&apos;;
$zip = new ZipArchive;
$zip-&gt;open($zipname, ZipArchive::CREATE);
foreach ($files as $file) {
  $zip-&gt;addFile($file);
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