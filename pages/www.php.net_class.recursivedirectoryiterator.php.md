# The RecursiveDirectoryIterator class



If you would like to get, say, all the *.php files in your project folder, recursively, you could use the following:<br><br>

```
<?php

$Directory = new RecursiveDirectoryIterator(&apos;path/to/project/&apos;);
$Iterator = new RecursiveIteratorIterator($Directory);
$Regex = new RegexIterator($Iterator, &apos;/^.+\.php$/i&apos;, RecursiveRegexIterator::GET_MATCH);

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
    $files[] = $info-&gt;getPathname();
  }
}
?>
```


1. RecursiveDirectoryIterator is just a RecursiveIterator that recurses into its children, until no more children are found.

2. The instantiation of RecursiveIteratorIterator causes RecursiveDirectoryIterator to *immediately* recurse infinitely into the entire filesystem tree (starting from the given base path).

3. Unnecessary filesystem recursion is slow.  In 90% of all cases, this is not what you want.

Remember this simple rule of thumb:

&#x2192; A RecursiveDirectoryIterator must be FILTERED or you have a solid reason for why it shouldn&apos;t.

On PHP &lt;5.4, implement the following - your custom conditions move into a proper filter:



```
<?php
$directory = new \RecursiveDirectoryIterator($path, \FilesystemIterator::FOLLOW_SYMLINKS);
$filter = new MyRecursiveFilterIterator($directory);
$iterator = new \RecursiveIteratorIterator($filter);
$files = array();
foreach ($iterator as $info) {
  $files[] = $info-&gt;getPathname();
}

class MyRecursiveFilterIterator extends \RecursiveFilterIterator {

  public function accept() {
    $filename = $this-&gt;current()-&gt;getFilename();
    // Skip hidden files and directories.
    if ($name[0] === &apos;.&apos;) {
      return FALSE;
    }
    if ($this-&gt;isDir()) {
      // Only recurse into intended subdirectories.
      return $name === &apos;wanted_dirname&apos;;
    }
    else {
      // Only consume files of interest.
      return strpos($name, &apos;wanted_filename&apos;) === 0;
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
  if ($current-&gt;getFilename()[0] === &apos;.&apos;) {
    return FALSE;
  }
  if ($current-&gt;isDir()) {
    // Only recurse into intended subdirectories.
    return $current-&gt;getFilename() === &apos;wanted_dirname&apos;;
  }
  else {
    // Only consume files of interest.
    return strpos($current-&gt;getFilename(), &apos;wanted_filename&apos;) === 0;
  }
});
$iterator = new \RecursiveIteratorIterator($filter);
$files = array();
foreach ($iterator as $info) {
  $files[] = $info-&gt;getPathname();
}
?>
```
<br><br>Have fun!  

#

Usage example:<br><br>

```
<?php

$path = realpath(&apos;/etc&apos;);

$objects = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($path), RecursiveIteratorIterator::SELF_FIRST);
foreach($objects as $name =&gt; $object){
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
   $path = $splFileInfo-&gt;isDir()
         ? array($splFileInfo-&gt;getFilename() =&gt; array())
         : array($splFileInfo-&gt;getFilename());

   for ($depth = $ritit-&gt;getDepth() - 1; $depth &gt;= 0; $depth--) {
       $path = array($ritit-&gt;getSubIterator($depth)-&gt;current()-&gt;getFilename() =&gt; $path);
   }
   $r = array_merge_recursive($r, $path);
}

print_r($r);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.recursivedirectoryiterator.php)

**[To root](/README.md)**