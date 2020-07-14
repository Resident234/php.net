# DOMDocument::createAttribute



Just in case it isn&apos;t clear (like I had), an example:<br><br>

```
<?php

$domDocument = new DOMDocument(&apos;1.0&apos;, "UTF-8");
$domElement = $domDocument-&gt;createElement(&apos;field&apos;,&apos;some random data&apos;);
$domAttribute = $domDocument-&gt;createAttribute(&apos;name&apos;);

// Value for the created attribute
$domAttribute-&gt;value = &apos;attributevalue&apos;;

// Don&apos;t forget to append it to the element
$domElement-&gt;appendChild($domAttribute);

// Append it to the document itself
$domDocument-&gt;appendChild($domElement);

?>
```


Will output:
&lt;?xml version="1.0" encoding="UTF-8"?>
```
<br>&lt;field name="attributevalue"&gt;some random data&lt;/field&gt;  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.createattribute.php)

**[To root](/README.md)**