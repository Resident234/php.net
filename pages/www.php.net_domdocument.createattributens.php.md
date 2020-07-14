# DOMDocument::createAttributeNS



If a new namespace is introduced while creating and inserting an attribute, createAttributeNS() does not behave in the same way as createElementNS().<br><br>(1) Location: With createAttributeNS(), the new namespace is declared at the level of the document element. By contrast, createElementNS() declares the new namespace at the level of the affected element itself.<br><br>(2) Timing: With createAttributeNS(), the new namespace is declared in the document as soon as the attribute is created - the attribute does not actually have to be inserted. createElementNS() doesn&apos;t affect the document as long as the element is not inserted.<br><br>An example:<br><br>

```
<?php
    
    $source = &lt;&lt;&lt;XML
&lt;?xml version="1.0" encoding="UTF-8"?>
```

&lt;root&gt;&lt;tag&gt;&lt;/tag&gt;&lt;/root&gt;
XML;
    
    /*
     
     I. createAttributeNS:
     * a new namespace shows up immediately, even without insertion of the attribute
     * the new namespace is declared at the level of the document element
     
    */
    
    $doc = new DOMDocument( &apos;1.0&apos; );
    $doc-&gt;loadXML( $source );
    
    // (1) We just create a "namespace&apos;d" attribute without appending it to any element.
    $attr_ns = $doc-&gt;createAttributeNS( &apos;{namespace_uri_here}&apos;, &apos;example:attr&apos; );
    
    print $doc-&gt;saveXML() . "\n";
    
    /*
      Result: The namespace declaration appears, having been added to the document element. Output:
      
      &lt;?xml version="1.0" encoding="UTF-8"?>
```

      &lt;root xmlns:example="{namespace_uri_here}"&gt;&lt;tag/&gt;&lt;/root&gt;
      
    */
    
    // (2) Next, we give the attribute a value and insert it.
    $attr_ns-&gt;value = &apos;value&apos;; 
    $doc-&gt;getElementsByTagName( &apos;tag&apos; )-&gt;item(0)-&gt;appendChild( $attr_ns );
    
    print $doc-&gt;saveXML() . "\n";
    
    /*
      Result: The "namespace&apos;d" attribute shows up as well. Output:
      
      &lt;?xml version="1.0" encoding="UTF-8"?>
```

      &lt;root xmlns:example="{namespace_uri_here}"&gt;&lt;tag example:attr="value"/&gt;&lt;/root&gt;
      
    */
    
    /*
     
     II. createElementNS:
     * a new namespace shows up only when the element is inserted
     * the new namespace is declared at the level of the inserted element
     
    */
    
    $doc = new DOMDocument( &apos;1.0&apos; );
    $doc-&gt;loadXML( $source );
    
    // (1) We create a "namespace&apos;d" element without inserting it into the document.
    $elem_ns = $doc-&gt;createElementNS( &apos;{namespace_uri_here}&apos;, &apos;example:newtag&apos; );
    
    print $doc-&gt;saveXML() . "\n";
    
    /*
      Result: The document remains unchanged. Output:
      
      &lt;?xml version="1.0" encoding="UTF-8"?>
```

      &lt;root&gt;&lt;tag/&gt;&lt;/root&gt;
      
    */
    
    // (2) Next, we insert the new element.
    $doc-&gt;getElementsByTagName( &apos;tag&apos; )-&gt;item(0)-&gt;appendChild( $elem_ns );
    
    print $doc-&gt;saveXML() . "\n";
    
    /*
      Result: The namespace declaration appears, and it is embedded in the element using it. Output:
      
      &lt;?xml version="1.0" encoding="UTF-8"?>
```

      &lt;root&gt;&lt;tag&gt;&lt;example:newtag xmlns:example="{namespace_uri_here}"/&gt;&lt;/tag&gt;&lt;/root&gt;
      
    */
    
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.createattributens.php)

**[To root](/README.md)**