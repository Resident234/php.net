# The DOMXPath class





```
<?php
// to retrieve selected html data, try these DomXPath examples:

$file = $DOCUMENT_ROOT. "test.html";
$doc = new DOMDocument();
$doc->loadHTMLFile($file);

$xpath = new DOMXpath($doc);

// example 1: for everything with an id
//$elements = $xpath->query("//*[@id]");

// example 2: for node data in a selected id
//$elements = $xpath->query("/html/body/div[@id='yourTagIdHere']");

// example 3: same as above with wildcard
$elements = $xpath->query("*/div[@id='yourTagIdHere']");

if (!is_null($elements)) {
  foreach ($elements as $element) {
    echo "<br/>[". $element->nodeName. "]";

    $nodes = $element->childNodes;
    foreach ($nodes as $node) {
      echo $node->nodeValue. "\n";
    }
  }
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.domxpath.php)

**[To root](/README.md)**