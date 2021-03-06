# DOMNode::appendChild



What&apos;s not mentioned here is that DOMNode::appendChild() can also be used to move an existing node to another part of the DOMDocument, e.g.<br><br>

```
<?php
$doc = new DOMDocument();
$doc->loadXML("<foobar><bar/><foo/></foobar>");
$bar = $doc->documentElement->firstChild;
$foo = $doc->documentElement->lastChild;
$foo->appendChild($bar);
print $doc->saveXML();
?>
```
<br><br>This produces:<br><br>``<?xml version="1.0"?>``;<br>&lt;foobar&gt;&lt;foo&gt;&lt;bar/&gt;&lt;/foo&gt;&lt;/foobar&gt;<br><br>Note that the nodes "&lt;foo/&gt;" and "&lt;bar/&gt;" were siblings, i.e. the first and last child of "&lt;foobar&gt;" but using appendChild() we were able to move "&lt;bar/&gt;" so that it is a child of "&lt;foo/&gt;".<br><br>This saves you the trouble of doing a DOMNode::removeChild($bar) to remove "&lt;bar/&gt;" before appending it as a child of "&lt;foo/&gt;". <br><br>Kris Dover  

---

[Official documentation page](https://www.php.net/manual/en/domnode.appendchild.php)

**[To root](/README.md)**