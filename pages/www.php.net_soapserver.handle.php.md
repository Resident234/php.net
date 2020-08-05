# SoapServer::handle



After much headache and looking through PHP source code, I finally found out why the handle() function would immediately send back a fault with the string "Bad Request".<br>Turns out that my client was sending valid XML, but the first line of the XML was the actual XML declaration:<br><br>``<?xml version="1.0" encoding="UTF-8" ?>``;<br><br>When the "handle" function in the SoapServer class is called, it first tries to parse the XML.  When the XML document can&apos;t be parsed, a "Bad Request" fault is returned and execution of the script immediately stops.  I assume that the XML parser built into PHP (libxml2) already assumes the document to be XML and when it finds the declaration, it thinks it isn&apos;t valid.<br><br>I added some XML parsing calls to my service before the handle() function is called to check for valid XML and avoid the "Bad Request" fault.  This also allows me to send back a more suitable error message:<br><br>

```
<?php
$parser = xml_parser_create("UTF-8");
if (!xml_parse($parser,$HTTP_RAW_POST_DATA,true)){
   $webService->fault("500", "Cannot parse XML: ".
      xml_error_string(xml_get_error_code($parser)).
       " at line: ".xml_get_current_line_number($parser).
       ", column: ".xml_get_current_column_number($parser));
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/soapserver.handle.php)

**[To root](/README.md)**