# DOMDocument::createAttributeNS



If a new namespace is introduced while creating and inserting an attribute, createAttributeNS() does not behave in the same way as createElementNS().<br><br>(1) Location: With createAttributeNS(), the new namespace is declared at the level of the document element. By contrast, createElementNS() declares the new namespace at the level of the affected element itself.<br><br>(2) Timing: With createAttributeNS(), the new namespace is declared in the document as soon as the attribute is created - the attribute does not actually have to be inserted. createElementNS() doesn&apos;t affect the document as long as the element is not inserted.<br><br>An example:<br><br>

```
<?php
    
    $source = <<<XML
``<?xml version="1.0" encoding="UTF-8" ?>``;
<root><tag></tag></root>
XML;
    
    /*
     
     I. createAttributeNS:
     * a new namespace shows up immediately, even without insertion of the attribute
     * the new namespace is declared at the level of the document element
     
    */
    
    $doc = new DOMDocument( '1.0' );
    $doc->loadXML( $source );
    
    // (1) We just create a "namespace'd" attribute without appending it to any element.
    $attr_ns = $doc->createAttributeNS( '{namespace_uri_here}', 'example:attr' );
    
    print $doc->saveXML() . "\n";
    
    /*
      Result: The namespace declaration appears, having been added to the document element. Output:
      
      ``<?xml version="1.0" encoding="UTF-8" ?>``;
      <root xmlns:example="{namespace_uri_here}"><tag/></root>
      
    */
    
    // (2) Next, we give the attribute a value and insert it.
    $attr_ns->value = 'value'; 
    $doc->getElementsByTagName( 'tag' )->item(0)->appendChild( $attr_ns );
    
    print $doc->saveXML() . "\n";
    
    /*
      Result: The "namespace'd" attribute shows up as well. Output:
      
      ``<?xml version="1.0" encoding="UTF-8" ?>``;
      <root xmlns:example="{namespace_uri_here}"><tag example:attr="value"/></root>
      
    */
    
    /*
     
     II. createElementNS:
     * a new namespace shows up only when the element is inserted
     * the new namespace is declared at the level of the inserted element
     
    */
    
    $doc = new DOMDocument( '1.0' );
    $doc->loadXML( $source );
    
    // (1) We create a "namespace'd" element without inserting it into the document.
    $elem_ns = $doc->createElementNS( '{namespace_uri_here}', 'example:newtag' );
    
    print $doc->saveXML() . "\n";
    
    /*
      Result: The document remains unchanged. Output:
      
      ``<?xml version="1.0" encoding="UTF-8" ?>``;
      <root><tag/></root>
      
    */
    
    // (2) Next, we insert the new element.
    $doc->getElementsByTagName( 'tag' )->item(0)->appendChild( $elem_ns );
    
    print $doc->saveXML() . "\n";
    
    /*
      Result: The namespace declaration appears, and it is embedded in the element using it. Output:
      
      ``<?xml version="1.0" encoding="UTF-8" ?>``;
      <root><tag><example:newtag xmlns:example="{namespace_uri_here}"/></tag></root>
      
    */
    
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/domdocument.createattributens.php)

**[To root](/README.md)**