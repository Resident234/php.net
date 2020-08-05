# The DirectoryIterator class



Shows us all files and catalogues in directory except "." and "..".<br><br>

```
<?php

foreach (new DirectoryIterator('../moodle') as $fileInfo) {
    if($fileInfo->isDot()) continue;
    echo $fileInfo->getFilename() . "<br>\n";
}

?>
```
  

---

Beware of the behavior when using FilesystemIterator::UNIX_PATHS, it&apos;s not applied as you might expect.<br><br>I guess this flag is added especially for use on windows.<br>However, the path you construct the RecursiveDirectoryIterator or FilesystemIterator with will not be available as a unix path.<br>I can&apos;t say this is a bug, since most methods are just purely inherited from DirectoryIterator.<br><br>In my test, I&apos;d expected a complete unix path. Unfortunately... not quite as expected:<br><br>

```
<?php
         // say $folder = C:\projects\lang

        $flags = FilesystemIterator::KEY_AS_PATHNAME | FilesystemIterator::CURRENT_AS_FILEINFO | FilesystemIterator::SKIP_DOTS | FilesystemIterator::UNIX_PATHS;
        $d_iterator = new RecursiveDirectoryIterator($folder, $flags);

        echo $d_iterator->getPath();

?>
```
<br><br>expected result: /projects/lang (or C:/projects/lang)<br>actual result: C:\projects\lang  

---

[Official documentation page](https://www.php.net/manual/en/class.directoryiterator.php)

**[To root](/README.md)**