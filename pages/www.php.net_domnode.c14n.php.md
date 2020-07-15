# DOMNode::C14N



C14N() returns an empty string if the node is not included in the document tree:<br>

```
<?php
$d = new DOMDocument('1.0');
$d->loadXML('<foo></foo>');
$n = $d->createElement('bar');
var_dump($n->C14N());
$d->documentElement->appendChild($n);
var_dump($n->C14N());
?>
```
<br>output:<br>string(0) ""<br>string(11) "&lt;bar&gt;&lt;/bar&gt;"  

#

When working with (malformed) HTML, you&apos;re probably better off using DOMDocument&apos;s saveHTML() method instead. C14N() will attempt to make your HTML valid XML, for example by converting &lt;br&gt; to &lt;br&gt;&lt;/br&gt;.<br><br>So instead of:<br>$html = $Node-&gt;C14N();<br><br>Use:<br>$html = $Node-&gt;ownerDocument-&gt;saveHTML( $Node );  

#

[Official documentation page](https://www.php.net/manual/en/domnode.c14n.php)

**[To root](/README.md)**