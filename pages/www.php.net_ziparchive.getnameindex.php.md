# ZipArchive::getNameIndex



I couldn&apos;t find any how-to example for getting the filenames, so I made an easy one.<br><br>Here&apos;s an example how to list all filenames from a zip-archive:<br><br>

```
<?php
$zip = new ZipArchive;
if ($zip->open('items.zip'))
{
     for($i = 0; $i &lt; $zip->numFiles; $i++)
     {   
          echo 'Filename: ' . $zip->getNameIndex($i) . '&lt;br /&gt;';
     }
}
else
{
     echo 'Error reading zip-archive!';
}
?>
```
<br><br>Hope it helps.  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.getnameindex.php)

**[To root](/README.md)**