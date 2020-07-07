# Phar::buildFromIterator





You have to set a flag on the RecursiveDirectoryIterator because by default, the current (&quot;.&quot;) and parent directory (&quot;..&quot;) are included in the listing. This leads to an error message similar to &quot;returned a path &quot;..&quot; that is not in the base directory&quot;.

To fix this, use &quot;SKIP_DOTS&quot;:



```
<?php
new RecursiveDirectoryIterator(
&#xA0; &#xA0; $srcRoot, FilesystemIterator::SKIP_DOTS
);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/phar.buildfromiterator.php)

**[To root](/README.md)**