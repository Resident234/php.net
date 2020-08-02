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
      if ($f != '.' &amp;&amp; $f != '..') {
        $filePath = "$folder/$f";
        // Remove prefix from file path before add to zip.
        $localPath = substr($filePath, $exclusiveLength);
        if (is_file($filePath)) {
          $zipFile->addFile($filePath, $localPath);
        } elseif (is_dir($filePath)) {
          // Add sub-directory.
          $zipFile->addEmptyDir($localPath);
          self::folderToZip($filePath, $zipFile, $exclusiveLength);
        }
      }
    }
    closedir($handle);
  }

  /**
   * Zip a folder (include itself).
   * Usage:
   *   HZip::zipDir('/path/to/sourceDir', '/path/to/out.zip');
   *
   * @param string $sourcePath Path of directory to be zip.
   * @param string $outZipPath Path of output zip file.
   */
  public static function zipDir($sourcePath, $outZipPath)
  {
    $pathInfo = pathInfo($sourcePath);
    $parentPath = $pathInfo['dirname'];
    $dirName = $pathInfo['basename'];

    $z = new ZipArchive();
    $z->open($outZipPath, ZIPARCHIVE::CREATE);
    $z->addEmptyDir($dirName);
    self::folderToZip($sourcePath, $z, strlen("$parentPath/"));
    $z->close();
  }
}
?>
```
  

---

With PHP 5.6+, you may come up with theses errors.<br><br>Warning: Unknown: Cannot destroy the zip context in Unknown on line 0<br><br>Warning: ZipArchive::close(): Can&apos;t remove file: No such file or directory in xxxx.php on line xx<br><br>Examples<br><br>Warning: Unknown: Cannot destroy the zip context in Unknown on line 0<br><br>

```
<?php         
    $za = new ZipArchive;
    $za->open('51-n.com.zip',ZipArchive::CREATE|ZipArchive::OVERWRITE);
?>
```


Warning: ZipArchive::close(): Can't remove file: No such file or directory in xxxx.php on line xx



```
<?php         
    $za = new ZipArchive;
    $za->open('51-n.com.zip',ZipArchive::CREATE|ZipArchive::OVERWRITE);
    $za->close();
?>
```


It happens when the zip archive is empty.
Your zip archive will not be saved on disk unless it has at least one file. What's more, when ZipArchive::OVERWRITE is applied, if there exists a file with the same name, it will be removed after ZipArchive::open() is called.

So, don't forget to put at least one file to your zip archive.



```
<?php         
    $za = new ZipArchive;
    $za->open('51-n.com.zip',ZipArchive::CREATE|ZipArchive::OVERWRITE);
    $za->addFromString('wuxiancheng.cn.txt','yes');
    $za->close();
?>
```
  

---

There is a usefull function to get the ZipArchive status as a human readable string :<br><br>

```
<?php
function ZipStatusString( $status )
{
    switch( (int) $status )
    {
        case ZipArchive::ER_OK           : return 'N No error';
        case ZipArchive::ER_MULTIDISK    : return 'N Multi-disk zip archives not supported';
        case ZipArchive::ER_RENAME       : return 'S Renaming temporary file failed';
        case ZipArchive::ER_CLOSE        : return 'S Closing zip archive failed';
        case ZipArchive::ER_SEEK         : return 'S Seek error';
        case ZipArchive::ER_READ         : return 'S Read error';
        case ZipArchive::ER_WRITE        : return 'S Write error';
        case ZipArchive::ER_CRC          : return 'N CRC error';
        case ZipArchive::ER_ZIPCLOSED    : return 'N Containing zip archive was closed';
        case ZipArchive::ER_NOENT        : return 'N No such file';
        case ZipArchive::ER_EXISTS       : return 'N File already exists';
        case ZipArchive::ER_OPEN         : return 'S Can\'t open file';
        case ZipArchive::ER_TMPOPEN      : return 'S Failure to create temporary file';
        case ZipArchive::ER_ZLIB         : return 'Z Zlib error';
        case ZipArchive::ER_MEMORY       : return 'N Malloc failure';
        case ZipArchive::ER_CHANGED      : return 'N Entry has been changed';
        case ZipArchive::ER_COMPNOTSUPP  : return 'N Compression method not supported';
        case ZipArchive::ER_EOF          : return 'N Premature EOF';
        case ZipArchive::ER_INVAL        : return 'N Invalid argument';
        case ZipArchive::ER_NOZIP        : return 'N Not a zip archive';
        case ZipArchive::ER_INTERNAL     : return 'N Internal error';
        case ZipArchive::ER_INCONS       : return 'N Zip archive inconsistent';
        case ZipArchive::ER_REMOVE       : return 'S Can\'t remove file';
        case ZipArchive::ER_DELETED      : return 'N Entry has been deleted';
        
        default: return sprintf('Unknown status %s', $status );
    }
}

?>
```
  

---

A way of zipping files and downloading them thereafter:<br>

```
<?php

 $files = array('image.jpeg','text.txt','music.wav');
$zipname = 'enter_any_name_for_the_zipped_file.zip';
$zip = new ZipArchive;
$zip->open($zipname, ZipArchive::CREATE);
foreach ($files as $file) {
  $zip->addFile($file);
}
$zip->close();

///Then download the zipped file.
header('Content-Type: application/zip');
header('Content-disposition: attachment; filename='.$zipname);
header('Content-Length: ' . filesize($zipname));
readfile($zipname);

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.ziparchive.php)

**[To root](/README.md)**