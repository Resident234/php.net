# Comparing generators with Iterator objects



This hardly seems a fair comparison between the two examples, size-for-size. As noted, generators are forward-only, meaning that it should be compared to an iterator with a dummy rewind function defined. Also, to be fair, since the iterator throws an exception, shouldn&apos;t the generator example also throw the same exception? The code comparison would become more like this:<br><br>

```
<?php
function getLinesFromFile($fileName) {
    if (!$fileHandle = fopen($fileName, 'r')) {
        throw new RuntimeException('Couldn\'t open file "' . $fileName . '"');
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
        if (!$this->fileHandle = fopen($fileName, 'r')) {
            throw new RuntimeException('Couldn\'t open file "' . $fileName . '"');
        }
    }
 
    public function rewind() { }
 
    public function valid() {
        return false !== $this->line;
    }
 
    public function current() {
        return $this->line;
    }
 
    public function key() {
        return $this->i;
    }
 
    public function next() {
        if (false !== $this->line) {
            $this->line = fgets($this->fileHandle);
            $this->i++;
        }
    }
 
    public function __destruct() {
        fclose($this->fileHandle);
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

if ($fileHandle = fopen($fileName, 'r')) {
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