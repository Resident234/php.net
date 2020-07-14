# The DOMXPath class





```
<?php
// to retrieve selected html data, try these DomXPath examples:

$file = $DOCUMENT_ROOT. "test.html";
$doc = new DOMDocument();
$doc-&gt;loadHTMLFile($file);

$xpath = new DOMXpath($doc);

// example 1: for everything with an id
//$elements = $xpath-&gt;query("//*[@id]");

// example 2: for node data in a selected id
//$elements = $xpath-&gt;query("/html/body/div[@id=&apos;yourTagIdHere&apos;]");

// example 3: same as above with wildcard
$elements = $xpath-&gt;query("*/div[@id=&apos;yourTagIdHere&apos;]");

if (!is_null($elements)) {
  foreach ($elements as $element) {
    echo "&lt;br/&gt;[". $element-&gt;nodeName. "]";

    $nodes = $element-&gt;childNodes;
    foreach ($nodes as $node) {
      echo $node-&gt;nodeValue. "\n";
    }
  }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.domxpath.php)

**[To root](/README.md)**