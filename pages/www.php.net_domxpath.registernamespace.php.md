# DOMXPath::registerNamespace



It is mentioned in a few places on the web, but it wasn&apos;t mentioned here. You need to use this function to set up a prefix for the default namespace of a document. <br><br>For instance, if you are trying to parse a Microsoft Spreadsheet XML file, which has the default namespace of "urn:schemas-microsoft-com:office:spreadsheet":<br><br>$doc = DOMDocument::load("my_spreadsheet.xml);<br>$xpath = new DOMXPath($doc);<br>$xpath-&gt;registerNamespace("m",<br>        "urn:schemas-microsoft-com:office:spreadsheet");<br>$query = &apos;/m:Workbook/m:Worksheet[1]/m:Table&apos;;<br>$result = $xpath-&gt;query($query, $doc);<br><br>You can use anything in place of the &apos;m&apos;, but you have to specify something! Just asking for "/Workbook/Worksheet/Table" doesn&apos;t work.  

---

[Official documentation page](https://www.php.net/manual/en/domxpath.registernamespace.php)

**[To root](/README.md)**