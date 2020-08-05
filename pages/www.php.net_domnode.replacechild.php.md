# DOMNode::replaceChild



If you are trying to replace more than one node at once, you have to be careful about iterating over the DOMNodeList.  If the old node has a different name from the new node, it will be removed from the list once it has been replaced.  Use a regressive loop:<br><br>

```
<?php

$xml = new DOMDocument;
$xml->load('docfile.xml');

$elements = $xml->getElementsByTagNameNS('http://www.example.com/NS/', '*');
$i = $elements->length - 1;
while ($i > -1) {
    $element = $elements->item($i);
    $ignore = false;

    $newelement = $xml>createTextNode('Some new node!');
    $element->parentNode->replaceChild($newelement, $element);
    $i--;
}

?>
```
<br><br>The loop counter ($i) will always be in the list&apos;s interval as removed elements indexes are above the counter.  

---

Here is a simple example for replacing a node:<br><br>Let&apos;s define our XML like so:<br><br>

```
<?php
$xml = <<<XML
``<?xml version="1.0"?>``;
<root>
  <parent>
     <child>bar</child>
     <child>foo</child>
  </parent>
</root>
XML;
?>
```


If we wanted to replace the entire <parent> node, we could do something like this:



```
<?php
// Create a new document fragment to hold the new <parent> node
$parent = new DomDocument;
$parent_node = $parent ->createElement('parent');

// Add some children
$parent_node->appendChild($parent->createElement('child', 'somevalue'));
$parent_node->appendChild($parent->createElement('child', 'anothervalue'));

// Add the keywordset into the new document
// The $parent variable now holds the new node as a document fragment
$parent->appendChild($parent_node);
?>
```


Next, we need to locate the old node:



```
<?php
// Load the XML
$dom = new DomDocument;
$dom->loadXML($xml);

// Locate the old parent node
$xpath = new DOMXpath($dom);
$nodelist = $xpath->query('/root/parent');
$oldnode = $nodelist->item(0);
?>
```


We then import and replace the new node:



```
<?php
// Load the $parent document fragment into the current document
$newnode = $dom->importNode($parent->documentElement, true);

// Replace
$oldnode->parentNode->replaceChild($newnode, $oldnode);

// Display
echo $dom->saveXML();
?>
```
<br><br>Our new node is successfully imported:<br><br>``<?xml version="1.0"?>``;<br>&lt;root&gt;<br>&lt;parent&gt;&lt;child&gt;somevalue&lt;/child&gt;&lt;child&gt;anothervalue&lt;/child&gt;&lt;/parent&gt;<br>&lt;/root&gt;  

---

[Official documentation page](https://www.php.net/manual/en/domnode.replacechild.php)

**[To root](/README.md)**