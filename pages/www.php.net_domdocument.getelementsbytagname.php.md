# DOMDocument::getElementsByTagName



Return if there are no matches is an empty DOMNodeList. Check using length property, e.g.:<br><br>

```
<?php
$nodes=$domDocument->getElementsByTagName('book') ; 
if ($nodes->length==0) {
   // no results
}
?>
```
  

#

Note that when using getElementsByTagName that it is a dynamic list. Thus if you have code which adjusts the DOM structure it will change the results of the getElementsByTagName results list.<br><br>The following code iterates through a complete set of results and changes them all to a new tag:<br><br>

```
<?php
 $nodes = $xml->getElementsByTagName ("oldtag");

 $nodeListLength = $nodes->length; // this value will also change
 for ($i = 0; $i < $nodeListLength; $i ++)
 {
    $node = $nodes->item(0);

    // some code to change the tag name from "oldtag" to something else
    // e.g. encrypting a tag element
 }
?>
```
<br><br>Since the list is dynamically updating, $nodes-&gt;item(0) is the next "unchanged" tag.  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.getelementsbytagname.php)

**[To root](/README.md)**