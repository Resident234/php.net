# DOMXPath::registerNamespace




<div class="phpcode"><span class="html">
It is mentioned in a few places on the web, but it wasn&apos;t mentioned here. You need to use this function to set up a prefix for the default namespace of a document. <br><br>For instance, if you are trying to parse a Microsoft Spreadsheet XML file, which has the default namespace of &quot;urn:schemas-microsoft-com:office:spreadsheet&quot;:<br><br>$doc = DOMDocument::load(&quot;my_spreadsheet.xml);<br>$xpath = new DOMXPath($doc);<br>$xpath-&gt;registerNamespace(&quot;m&quot;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &quot;urn:schemas-microsoft-com:office:spreadsheet&quot;);<br>$query = &apos;/m:Workbook/m:Worksheet[1]/m:Table&apos;;<br>$result = $xpath-&gt;query($query, $doc);<br><br>You can use anything in place of the &apos;m&apos;, but you have to specify something! Just asking for &quot;/Workbook/Worksheet/Table&quot; doesn&apos;t work.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domxpath.registernamespace.php)

**[⬆ to root](/)**