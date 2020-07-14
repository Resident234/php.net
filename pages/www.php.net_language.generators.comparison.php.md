# Comparing generators with Iterator objects



This hardly seems a fair comparison between the two examples, size-for-size. As noted, generators are forward-only, meaning that it should be compared to an iterator with a dummy rewind function defined. Also, to be fair, since the iterator throws an exception, shouldn&apos;t the generator example also throw the same exception? The code comparison would become more like this:<br><br>

```
<?php
function getLinesFromFile($fileName) {
    if (!$fileHandle = fopen($fileName, &apos;r&apos;)) {
        throw new RuntimeException(&apos;Couldn\&apos;t open file "&apos; . $fileName . &apos;"&apos;);
    }
 
    while (false !== $line = fgets($fileHandle)) {
        yield $line;
    }
 
    fclose($fileHandle);
}

// versus...

class LineIterator implements Iterator {
    protected $fileHandle;
 
    protected $line;
    protected $i;
 
    public function __construct($fileName) {
        if (!$this-&gt;fileHandle = fopen($fileName, &apos;r&apos;)) {
            throw new RuntimeException(&apos;Couldn\&apos;t open file "&apos; . $fileName . &apos;"&apos;);
        }
    }
 
    public function rewind() { }
 
    public function valid() {
        return false !== $this-&gt;line;
    }
 
    public function current() {
        return $this-&gt;line;
    }
 
    public function key() {
        return $this-&gt;i;
    }
 
    public function next() {
        if (false !== $this-&gt;line) {
            $this-&gt;line = fgets($this-&gt;fileHandle);
            $this-&gt;i++;
        }
    }
 
    public function __destruct() {
        fclose($this-&gt;fileHandle);
    }
}
?>
```
<br><br>The generator is still obviously much shorter, but this seems a more reasonable comparison.  

#

I think that this is bad generator example.<br>If user will not consume all lines then file will not be closed.<br><br>

```
<?php
function getLinesFromFile($fileHandle) {
    while (false !== $line = fgets($fileHandle)) {
        yield $line;
    }
}

if ($fileHandle = fopen($fileName, &apos;r&apos;)) {
    /*
    something with getLinesFromFile
    */
    fclose($fileHandle);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.generators.comparison.php)

**[To root](/README.md)**