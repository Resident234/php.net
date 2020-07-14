# SimpleXML Functions



If you are having trouble accessing CDATA in your simplexml document, you don&apos;t need to str_replace/preg_replace the CDATA out before loading it with simplexml.<br><br>You can do this instead, and all your CDATA contents will be merged into the element contents as strings.<br><br>$xml = simplexml_load_file($xmlfile,<br>&apos;SimpleXMLElement&apos;, LIBXML_NOCDATA);  

#

[Official documentation page](https://www.php.net/manual/en/ref.simplexml.php)

**[To root](/README.md)**