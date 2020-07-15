# Examples



Here&apos;s a very simple example on how to use PHP5 to transform a XML file using a XSL file.<br><br>

```
<?php

   $xslDoc = new DOMDocument();
   $xslDoc->load("collection.xsl");

   $xmlDoc = new DOMDocument();
   $xmlDoc->load("collection.xml");

   $proc = new XSLTProcessor();
   $proc->importStylesheet($xslDoc);
   echo $proc->transformToXML($xmlDoc);

?>
```
<br><br>For the sake of simplicity there&apos;s no error handling on this code. I hope this helps.  

#

[Official documentation page](https://www.php.net/manual/en/xsl.examples.php)

**[To root](/README.md)**