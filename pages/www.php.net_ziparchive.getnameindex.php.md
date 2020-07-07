# ZipArchive::getNameIndex





I couldn&apos;t find any how-to example for getting the filenames, so I made an easy one.



Here&apos;s an example how to list all filenames from a zip-archive:





```
<?php

$zip = new ZipArchive;

if ($zip-&gt;open(&apos;items.zip&apos;))

{

&#xA0; &#xA0;&#xA0; for($i = 0; $i &lt; $zip-&gt;numFiles; $i++)

&#xA0; &#xA0;&#xA0; {&#xA0;&#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Filename: &apos; . $zip-&gt;getNameIndex($i) . &apos;&lt;br /&gt;&apos;;

&#xA0; &#xA0;&#xA0; }

}

else

{

&#xA0; &#xA0;&#xA0; echo &apos;Error reading zip-archive!&apos;;

}

?>
```




Hope it helps.

  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.getnameindex.php)

**[To root](/README.md)**