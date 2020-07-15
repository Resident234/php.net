# The DOMElement class



Caveat!<br>It took me almost an hour to figure this out, so I hope it saves at least one of you some time.<br><br>If you want to debug your DOM tree and try var_dump() or similar you will be fooled into thinking the DOMElement that you are looking at is empty, because var_dump() says: object(DOMElement)#1 (0) { } <br><br>After much debugging I found out that all DOM objects are invisible to var_dump() and print_r(), my guess is because they are C objects and not PHP objects. So I tried saveXML(), which works fine on DOMDocument, but is not implemented on DOMElement.<br><br>The solution is simple (if you know it):<br>$xml = $domElement-&gt;ownerDocument-&gt;saveXML($domElement);<br><br>This will give you an XML representation of $domElement.  

#

Hi to get the value of DOMElement just get the nodeValue public parameter (it is inherited from DOMNode):<br>

```
<?php 
echo $domElement->nodeValue; 
?>
```
<br>Everything is obvious if you now about this thing ;-)  

#

This page doesn&apos;t list the inherited properties from DOMNode, e.g. the quite important textContent property. It would be immensely helpful if it would list those as well.  

#

Hi!<br><br>Combining all th comments, the easiest way to get inner HTML of the node is to use this function:<br><br>

```
<?php
function get_inner_html( $node ) {
    $innerHTML= '';
    $children = $node->childNodes;
    foreach ($children as $child) {
        $innerHTML .= $child->ownerDocument->saveXML( $child );
    }

    return $innerHTML;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.domelement.php)

**[To root](/README.md)**