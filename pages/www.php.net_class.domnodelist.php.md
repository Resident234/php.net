# The DOMNodeList class



If you want to recurse over a DOM then this might help: <br>

```
<?php 

/**
 * PHP's DOM classes are recursive but don't provide an implementation of 
 * RecursiveIterator. This class provides a RecursiveIterator for looping over DOMNodeList
 */
class DOMNodeRecursiveIterator extends ArrayIterator implements RecursiveIterator {
  
  public function __construct (DOMNodeList $node_list) {
    
    $nodes = array();
    foreach($node_list as $node) {
      $nodes[] = $node;
    }
    
    parent::__construct($nodes);
    
  }
  
  public function getRecursiveIterator(){
    return new RecursiveIteratorIterator($this, RecursiveIteratorIterator::SELF_FIRST);
  }
  
  public function hasChildren () {
    return $this->current()->hasChildNodes();
  }

  
  public function getChildren () {
    return new self($this->current()->childNodes);
  }
  
}
?>
```
  

---

You can modify, and even delete, nodes from a DOMNodeList if you iterate backwards:<br><br>$els = $document-&gt;getElementsByTagName(&apos;input&apos;);<br>for ($i = $els-&gt;length; --$i &gt;= 0; ) {<br>  $el = $els-&gt;item($i);<br>  switch ($el-&gt;getAttribute(&apos;name&apos;)) {<br>    case &apos;MAX_FILE_SIZE&apos; :<br>      $el-&gt;parentNode-&gt;removeChild($el);<br>    break;<br>    case &apos;inputfile&apos; :<br>      $el-&gt;setAttribute(&apos;type&apos;, &apos;text&apos;);<br>    //break;<br>  }<br>}  

---

Note that $length is calculated (php5.3.2).<br>Iterating over a large NodeList may be time expensive.<br><br>Prefer :<br>$nb = $nodelist-&gt;length;<br>for($pos=0; $pos&lt;$nb; $pos++)<br><br>Than:<br>for($pos=0; $pos&lt;$nodelist-&gt;length; $pos++)<br><br>I had a hard time figuring that out...  

---

[Official documentation page](https://www.php.net/manual/en/class.domnodelist.php)

**[To root](/README.md)**