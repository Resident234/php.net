# Comparing generators with Iterator objects





This hardly seems a fair comparison between the two examples, size-for-size. As noted, generators are forward-only, meaning that it should be compared to an iterator with a dummy rewind function defined. Also, to be fair, since the iterator throws an exception, shouldn&apos;t the generator example also throw the same exception? The code comparison would become more like this:



```
<?php
function getLinesFromFile($fileName) {
&#xA0; &#xA0; if (!$fileHandle = fopen($fileName, &apos;r&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Couldn\&apos;t open file &quot;&apos; . $fileName . &apos;&quot;&apos;);
&#xA0; &#xA0; }
 
&#xA0; &#xA0; while (false !== $line = fgets($fileHandle)) {
&#xA0; &#xA0; &#xA0; &#xA0; yield $line;
&#xA0; &#xA0; }
 
&#xA0; &#xA0; fclose($fileHandle);
}

// versus...

class LineIterator implements Iterator {
&#xA0; &#xA0; protected $fileHandle;
 
&#xA0; &#xA0; protected $line;
&#xA0; &#xA0; protected $i;
 
&#xA0; &#xA0; public function __construct($fileName) {
&#xA0; &#xA0; &#xA0; &#xA0; if (!$this-&gt;fileHandle = fopen($fileName, &apos;r&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Couldn\&apos;t open file &quot;&apos; . $fileName . &apos;&quot;&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public function rewind() { }
 
&#xA0; &#xA0; public function valid() {
&#xA0; &#xA0; &#xA0; &#xA0; return false !== $this-&gt;line;
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public function current() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;line;
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public function key() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;i;
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public function next() {
&#xA0; &#xA0; &#xA0; &#xA0; if (false !== $this-&gt;line) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;line = fgets($this-&gt;fileHandle);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;i++;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public function __destruct() {
&#xA0; &#xA0; &#xA0; &#xA0; fclose($this-&gt;fileHandle);
&#xA0; &#xA0; }
}
?>
```


The generator is still obviously much shorter, but this seems a more reasonable comparison.

  

#



I think that this is bad generator example.
If user will not consume all lines then file will not be closed.



```
<?php
function getLinesFromFile($fileHandle) {
&#xA0; &#xA0; while (false !== $line = fgets($fileHandle)) {
&#xA0; &#xA0; &#xA0; &#xA0; yield $line;
&#xA0; &#xA0; }
}

if ($fileHandle = fopen($fileName, &apos;r&apos;)) {
&#xA0; &#xA0; /*
&#xA0; &#xA0; something with getLinesFromFile
&#xA0; &#xA0; */
&#xA0; &#xA0; fclose($fileHandle);
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.generators.comparison.php)

**[To root](/README.md)**