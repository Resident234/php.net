# ZipArchive::getNameIndex



I couldn&apos;t find any how-to example for getting the filenames, so I made an easy one.<br><br>Here&apos;s an example how to list all filenames from a zip-archive:<br><br>

```
<?php
$zip = new ZipArchive;
if ($zip-&gt;open(&apos;items.zip&apos;))
{
     for($i = 0; $i &lt; $zip-&gt;numFiles; $i++)
     {   
          echo &apos;Filename: &apos; . $zip-&gt;getNameIndex($i) . &apos;&lt;br /&gt;&apos;;
     }
}
else
{
     echo &apos;Error reading zip-archive!&apos;;
}
?>
```
<br><br>Hope it helps.  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.getnameindex.php)

**[To root](/README.md)**