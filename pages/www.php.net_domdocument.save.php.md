# DOMDocument::save



The XML parser converts the text of an XML document into UTF-8, even if you have set the character encoding of the XML, for example as a second parameter of the DOMDocument constructor. After parsing the XML with the load() command all its texts have been converted to UTF-8.<br><br>In case you append text nodes with special characters (e. g. Umlaut) to your XML document you should therefore use utf8_encode() with your text to convert it into UTF-8 before you append the text to the document. Otherwise you will get an error message like "output conversion failed due to conv error" at the save() command. See example below:<br><br>

```
<?php
// Text to insert into XML below
$txt = "a text with special characters like '&#xE4;', '&#xDF;', '&#xDC;' etc.";

// Create Instance of DOMDocument
$dom =  new DOMDocument;
// Load XML file
// Was created before with DOMDocument('1.0', 'iso-8859-1')
$dom = $dom->load("file.xml");
// Find the parent node
$parent = $dom->documentElement;
// Create Instance of DomXPath
$xpath = new DomXPath($dom);
// new node will be inserted before this node
$next = $xpath->query("//parentnode/childnode");
// Create the new element
$new_elem = $dom->createElement('new_elem');
// Insert the new element
$parent->insertBefore($new_elem, $next->item(0));
// DOMXML = utf-8! (will be converted to iso-8859-1 only at 'save()')
// prevents error message "output conversion failed due to conv error" at 'save()'
$txt = utf8_encode($txt);
// Create new text node with utf-8 encoded string
$nodetext = $dom->createTextNode("$txt");
// Append text node to new element
$nodetext = $new_elem->appendChild($nodetext);
// save 
$dom->save("file.xml");
?>
```
<br><br>Hope this helps someone.<br><br>siegparr  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.save.php)

**[To root](/README.md)**