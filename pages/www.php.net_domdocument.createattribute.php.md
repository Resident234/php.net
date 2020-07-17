# DOMDocument::createAttribute



Just in case it isn&apos;t clear (like I had), an example:<br><br>

```
<?php

$domDocument = new DOMDocument('1.0', "UTF-8");
$domElement = $domDocument->createElement('field','some random data');
$domAttribute = $domDocument->createAttribute('name');

// Value for the created attribute
$domAttribute->value = 'attributevalue';

// Don't forget to append it to the element
$domElement->appendChild($domAttribute);

// Append it to the document itself
$domDocument->appendChild($domElement);

?>
```


Will output:
<?xml version="1.0" encoding="UTF-8"?>
```
<br>&lt;field name="attributevalue"&gt;some random data&lt;/field&gt;  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.createattribute.php)

**[To root](/README.md)**