# DOMNode::replaceChild



If you are trying to replace more than one node at once, you have to be careful about iterating over the DOMNodeList.  If the old node has a different name from the new node, it will be removed from the list once it has been replaced.  Use a regressive loop:<br><br>

```
<?php

$xml = new DOMDocument;
$xml-&gt;load(&apos;docfile.xml&apos;);

$elements = $xml-&gt;getElementsByTagNameNS(&apos;http://www.example.com/NS/&apos;, &apos;*&apos;);
$i = $elements-&gt;length - 1;
while ($i &gt; -1) {
    $element = $elements-&gt;item($i);
    $ignore = false;

    $newelement = $xml&gt;createTextNode(&apos;Some new node!&apos;);
    $element-&gt;parentNode-&gt;replaceChild($newelement, $element);
    $i--;
}

?>
```
<br><br>The loop counter ($i) will always be in the list&apos;s interval as removed elements indexes are above the counter.  

#

Here is a simple example for replacing a node:<br><br>Let&apos;s define our XML like so:<br><br>

```
<?php
$xml = &lt;&lt;&lt;XML
&lt;?xml version="1.0"?>
```

&lt;root&gt;
  &lt;parent&gt;
     &lt;child&gt;bar&lt;/child&gt;
     &lt;child&gt;foo&lt;/child&gt;
  &lt;/parent&gt;
&lt;/root&gt;
XML;
?>
```


If we wanted to replace the entire &lt;parent&gt; node, we could do something like this:



```
<?php
// Create a new document fragment to hold the new &lt;parent&gt; node
$parent = new DomDocument;
$parent_node = $parent -&gt;createElement(&apos;parent&apos;);

// Add some children
$parent_node-&gt;appendChild($parent-&gt;createElement(&apos;child&apos;, &apos;somevalue&apos;));
$parent_node-&gt;appendChild($parent-&gt;createElement(&apos;child&apos;, &apos;anothervalue&apos;));

// Add the keywordset into the new document
// The $parent variable now holds the new node as a document fragment
$parent-&gt;appendChild($parent_node);
?>
```


Next, we need to locate the old node:



```
<?php
// Load the XML
$dom = new DomDocument;
$dom-&gt;loadXML($xml);

// Locate the old parent node
$xpath = new DOMXpath($dom);
$nodelist = $xpath-&gt;query(&apos;/root/parent&apos;);
$oldnode = $nodelist-&gt;item(0);
?>
```


We then import and replace the new node:



```
<?php
// Load the $parent document fragment into the current document
$newnode = $dom-&gt;importNode($parent-&gt;documentElement, true);

// Replace
$oldnode-&gt;parentNode-&gt;replaceChild($newnode, $oldnode);

// Display
echo $dom-&gt;saveXML();
?>
```


Our new node is successfully imported:

&lt;?xml version="1.0"?>
```
<br>&lt;root&gt;<br>&lt;parent&gt;&lt;child&gt;somevalue&lt;/child&gt;&lt;child&gt;anothervalue&lt;/child&gt;&lt;/parent&gt;<br>&lt;/root&gt;  

#

[Official documentation page](https://www.php.net/manual/en/domnode.replacechild.php)

**[To root](/README.md)**