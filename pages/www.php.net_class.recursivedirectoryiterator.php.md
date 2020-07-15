# The RecursiveDirectoryIterator class



If you would like to get, say, all the *.php files in your project folder, recursively, you could use the following:<br><br>

```
<?php

$Directory = new RecursiveDirectoryIterator('path/to/project/');
$Iterator = new RecursiveIteratorIterator($Directory);
$Regex = new RegexIterator($Iterator, '/^.+\.php$/i', RecursiveRegexIterator::GET_MATCH);

?>
```
<br><br>$Regex will contain a single index array for each PHP file.  

#

Since I continue to run into implementations across the net that are unintentionally running into this trap &#x2014; beware:<br><br>RecursiveDirectoryIterator recurses without limitations into the full filesystem tree.<br><br>Do NOT do the following, unless you intentionally want to infinitely recurse without limitations:<br><br>

```
<?php
$directory = new \RecursiveDirectoryIterator($path);
$iterator = new \RecursiveIteratorIterator($directory);
$files = array();
foreach ($iterator as $info) {
  if (...custom conditions...) {
    $files[] = $info->getPathname();
  }
}
?>
```


1. RecursiveDirectoryIterator is just a RecursiveIterator that recurses into its children, until no more children are found.

2. The instantiation of RecursiveIteratorIterator causes RecursiveDirectoryIterator to *immediately* recurse infinitely into the entire filesystem tree (starting from the given base path).

3. Unnecessary filesystem recursion is slow.  In 90% of all cases, this is not what you want.

Remember this simple rule of thumb:

&#x2192; A RecursiveDirectoryIterator must be FILTERED or you have a solid reason for why it shouldn't.

On PHP <5.4, implement the following - your custom conditions move into a proper filter:



```
<?php
$directory = new \RecursiveDirectoryIterator($path, \FilesystemIterator::FOLLOW_SYMLINKS);
$filter = new MyRecursiveFilterIterator($directory);
$iterator = new \RecursiveIteratorIterator($filter);
$files = array();
foreach ($iterator as $info) {
  $files[] = $info->getPathname();
}

class MyRecursiveFilterIterator extends \RecursiveFilterIterator {

  public function accept() {
    $filename = $this->current()->getFilename();
    // Skip hidden files and directories.
    if ($name[0] === '.') {
      return FALSE;
    }
    if ($this->isDir()) {
      // Only recurse into intended subdirectories.
      return $name === 'wanted_dirname';
    }
    else {
      // Only consume files of interest.
      return strpos($name, 'wanted_filename') === 0;
    }
  }

}
?>
```


On PHP 5.4+, PHP core addressed the slightly cumbersome issue of having to create an entirely new class and you can leverage the new RecursiveCallbackFilterIterator instead:



```
<?php
$directory = new \RecursiveDirectoryIterator($path, \FilesystemIterator::FOLLOW_SYMLINKS);
$filter = new \RecursiveCallbackFilterIterator($directory, function ($current, $key, $iterator) {
  // Skip hidden files and directories.
  if ($current->getFilename()[0] === '.') {
    return FALSE;
  }
  if ($current->isDir()) {
    // Only recurse into intended subdirectories.
    return $current->getFilename() === 'wanted_dirname';
  }
  else {
    // Only consume files of interest.
    return strpos($current->getFilename(), 'wanted_filename') === 0;
  }
});
$iterator = new \RecursiveIteratorIterator($filter);
$files = array();
foreach ($iterator as $info) {
  $files[] = $info->getPathname();
}
?>
```
<br><br>Have fun!  

#

Usage example:<br><br>

```
<?php

$path = realpath('/etc');

$objects = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($path), RecursiveIteratorIterator::SELF_FIRST);
foreach($objects as $name => $object){
    echo "$name\n";
}

?>
```
<br><br>This prints a list of all files and directories under $path (including $path ifself). If you want to omit directories, remove the RecursiveIteratorIterator::SELF_FIRST part.  

#

If you need to convert a nested directory tree into a multidimensional array, use this code:<br><br>

```
<?php
$ritit = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($startpath), RecursiveIteratorIterator::CHILD_FIRST);
$r = array();
foreach ($ritit as $splFileInfo) {
   $path = $splFileInfo->isDir()
         ? array($splFileInfo->getFilename() => array())
         : array($splFileInfo->getFilename());

   for ($depth = $ritit->getDepth() - 1; $depth >= 0; $depth--) {
       $path = array($ritit->getSubIterator($depth)->current()->getFilename() => $path);
   }
   $r = array_merge_recursive($r, $path);
}

print_r($r);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.recursivedirectoryiterator.php)

**[To root](/README.md)**