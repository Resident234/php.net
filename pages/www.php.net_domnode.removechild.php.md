# DOMNode::removeChild



You can&apos;t remove DOMNodes from a DOMNodeList as you&apos;re iterating over them in a foreach loop. For example...<br><br>

```
<?php
$domNodeList = $domDocument->getElementsByTagname('p');
foreach ( $domNodeList as $domElement ) {
  //  ...do stuff with $domElement...
  $domElement->parentNode->removeChild($domElement);
}
?>
```


... will seemingly leave the internal iterator on the foreach out of wack and results will be quite strange. Though, making a queue of items to remove seems to work. For example...



```
<?php
$domNodeList = $domDocument->getElementsByTagname('p');
$domElemsToRemove = array();
foreach ( $domNodeList as $domElement ) {
  // ...do stuff with $domElement...
  $domElemsToRemove[] = $domElement;
}
foreach( $domElemsToRemove as $domElement ){
  $domElement->parentNode->removeChild($domElement);
}
?>
```
  

#

These two functions might be helpful to anyone trying to delete a node and all of its children:<br><br>

```
<?php
function deleteNode($node) {
    deleteChildren($node);
    $parent = $node->parentNode;
    $oldnode = $parent->removeChild($node);
}

function deleteChildren($node) {
    while (isset($node->firstChild)) {
        deleteChildren($node->firstChild);
        $node->removeChild($node->firstChild);
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/domnode.removechild.php)

**[To root](/README.md)**