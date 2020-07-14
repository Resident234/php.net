# The DOMNode class



It took me forever to find a mapping for the XML_*_NODE constants. So I thought, it&apos;d be handy to paste it here:<br><br> 1 XML_ELEMENT_NODE<br> 2 XML_ATTRIBUTE_NODE<br> 3 XML_TEXT_NODE<br> 4 XML_CDATA_SECTION_NODE<br> 5 XML_ENTITY_REFERENCE_NODE<br> 6 XML_ENTITY_NODE<br> 7 XML_PROCESSING_INSTRUCTION_NODE<br> 8 XML_COMMENT_NODE<br> 9 XML_DOCUMENT_NODE<br>10 XML_DOCUMENT_TYPE_NODE<br>11 XML_DOCUMENT_FRAGMENT_NODE<br>12 XML_NOTATION_NODE  

#

You cannot simply overwrite $textContent, to replace the text content of a DOMNode, as the missing readonly flag suggests. Instead you have to do something like this:<br><br>

```
<?php

$node-&gt;removeChild($node-&gt;firstChild);
$node-&gt;appendChild(new DOMText(&apos;new text content&apos;));

?>
```


This example shows what happens:



```
<?php

$doc = DOMDocument::loadXML(&apos;&lt;node&gt;old content&lt;/node&gt;&apos;);
$node = $doc-&gt;getElementsByTagName(&apos;node&apos;)-&gt;item(0);
echo "Content 1: ".$node-&gt;textContent."\n";

$node-&gt;textContent = &apos;new content&apos;;
echo "Content 2: ".$node-&gt;textContent."\n";

$newText = new DOMText(&apos;new content&apos;);

$node-&gt;appendChild($newText);
echo "Content 3: ".$node-&gt;textContent."\n";

$node-&gt;removeChild($node-&gt;firstChild);
$node-&gt;appendChild($newText);
echo "Content 4: ".$node-&gt;textContent."\n";

?>
```


The output is:

Content 1: old content // starting content
Content 2: old content // trying to replace overwriting $node-&gt;textContent
Content 3: old contentnew content // simply appending the new text node
Content 4: new content // removing firstchild before appending the new text node

If you want to have a CDATA section, use this:



```
<?php
$doc = DOMDocument::loadXML(&apos;&lt;node&gt;old content&lt;/node&gt;&apos;);
$node = $doc-&gt;getElementsByTagName(&apos;node&apos;)-&gt;item(0);
$node-&gt;removeChild($node-&gt;firstChild);
$newText = $doc-&gt;createCDATASection(&apos;new cdata content&apos;);
$node-&gt;appendChild($newText);
echo "Content withCDATA: ".$doc-&gt;saveXML($node)."\n";
?>
```
  

#

For clarification:<br>The assumingly &apos;discoverd&apos; by previous posters and seemingly undocumented methods (.getElementsByTagName and .getAttribute) on this class (DOMNode) are in fact methods of the class DOMElement, which inherits from DOMNode.<br><br>See: http://www.php.net/manual/en/class.domelement.php  

#

[Official documentation page](https://www.php.net/manual/en/class.domnode.php)

**[To root](/README.md)**