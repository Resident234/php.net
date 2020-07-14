# Predefined Constants



When inserting XML DOM Elements inside existing XML DOM Elements that I loaded from an XML file using the following code, none of my new elements were formatted correctly, they just showed up on one line:<br><br>

```
<?php 
$dom = DOMDocument::load(&apos;file.xml&apos;); 
$dom-&gt;formatOutput = true;
//$dom-&gt;add some new elements with child nodes somewhere inside the loaded XML using insertBefore();
$dom-&gt;saveXML();
//output: everything looks normal but the new nodes are all on one line.
?>
```


I found I could pass LIBXML_NOBLANKS to the load method and it would reformat the whole document, including my added stuff:


```
<?php 
$dom = DOMDocument::load(&apos;file.xml&apos;, LIBXML_NOBLANKS); 
$dom-&gt;formatOutput = true;
//$dom-&gt;add some new elements with child nodes somewhere inside the loaded XML using insertBefore();
$dom-&gt;saveXML();
//output: everything looks newly formatted, including new nodes
?>
```
<br><br>Hope this helps, took me hours of trial and error to figure this out!  

#

[Official documentation page](https://www.php.net/manual/en/libxml.constants.php)

**[To root](/README.md)**