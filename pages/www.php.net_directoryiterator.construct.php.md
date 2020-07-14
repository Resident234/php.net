# DirectoryIterator::__construct



Here&apos;s all-in-one DirectoryIterator:<br><br>

```
<?php

/**
 * Real Recursive Directory Iterator
 */
class RRDI extends RecursiveIteratorIterator {
  /**
   * Creates Real Recursive Directory Iterator
   * @param string $path
   * @param int $flags
   * @return DirectoryIterator
   */
  public function __construct($path, $flags = 0) {
    parent::__construct(new RecursiveDirectoryIterator($path, $flags));
  }
}

/**
 * Real RecursiveDirectoryIterator Filtered Class
 * Returns only those items which filenames match given regex
 */
class AdvancedDirectoryIterator extends FilterIterator {
  /**
   * Regex storage
   * @var string
   */
  private $regex;
  /**
   * Creates new AdvancedDirectoryIterator
   * @param string $path, prefix with &apos;-R &apos; for recursive, postfix with /[wildcards] for matching
   * @param int $flags
   * @return DirectoryIterator
   */
  public function  __construct($path, $flags = 0) {
    if (strpos($path, &apos;-R &apos;) === 0) { $recursive = true; $path = substr($path, 3); }
    if (preg_match(&apos;~/?([^/]*\*[^/]*)$~&apos;, $path, $matches)) { // matched wildcards in filename
      $path = substr($path, 0, -strlen($matches[1]) - 1); // strip wildcards part from path
      $this-&gt;regex = &apos;~^&apos; . str_replace(&apos;*&apos;, &apos;.*&apos;, str_replace(&apos;.&apos;, &apos;\.&apos;, $matches[1])) . &apos;$~&apos;; // convert wildcards to regex 
      if (!$path) $path = &apos;.&apos;; // if no path given, we assume CWD
    }
    parent::__construct($recursive ? new RRDI($path, $flags) : new DirectoryIterator($path));
  }
  /**
   * Checks for regex in current filename, or matches all if no regex specified
   * @return bool
   */
  public function accept() { // FilterIterator method
    return $this-&gt;regex === null ? true : preg_match($this-&gt;regex, $this-&gt;getInnerIterator()-&gt;getFilename());
  }
}

?>
```


Some examples:



```
<?php

/* @var $i DirectoryIterator */

foreach (new AdvancedDirectoryIterator(&apos;.&apos;) as $i) echo $i-&gt;getPathname() . &apos;&lt;br/&gt;&apos;;
// will output all files and directories in CWD

foreach (new AdvancedDirectoryIterator(&apos;-R *.php&apos;) as $i) echo $i-&gt;getPathname() . &apos;&lt;br/&gt;&apos;;
// will output all php files in CWD and all subdirectories

foreach (new AdvancedDirectoryIterator(&apos;-R js/jquery-*.js&apos;) as $i) echo $i-&gt;getPathname() . &apos;&lt;br/&gt;&apos;;
// will output all jQuery versions in directory js, or throw an exception if directory js doesn&apos;t exist

?>
```
<br><br>Pretty cool, huh? :)  

#

[Official documentation page](https://www.php.net/manual/en/directoryiterator.construct.php)

**[To root](/README.md)**