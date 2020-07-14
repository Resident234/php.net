# Phar::buildFromIterator



You have to set a flag on the RecursiveDirectoryIterator because by default, the current (".") and parent directory ("..") are included in the listing. This leads to an error message similar to "returned a path ".." that is not in the base directory".<br><br>To fix this, use "SKIP_DOTS":<br><br>

```
<?php
new RecursiveDirectoryIterator(
    $srcRoot, FilesystemIterator::SKIP_DOTS
);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/phar.buildfromiterator.php)

**[To root](/README.md)**