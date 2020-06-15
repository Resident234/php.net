# SoapServer::handle




<div class="phpcode"><span class="html">
After much headache and looking through PHP source code, I finally found out why the handle() function would immediately send back a fault with the string &quot;Bad Request&quot;.
<br>Turns out that my client was sending valid XML, but the first line of the XML was the actual XML declaration:
<br>
<br>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
<br>
<br>When the &quot;handle&quot; function in the SoapServer class is called, it first tries to parse the XML.&#xA0; When the XML document can&apos;t be parsed, a &quot;Bad Request&quot; fault is returned and execution of the script immediately stops.&#xA0; I assume that the XML parser built into PHP (libxml2) already assumes the document to be XML and when it finds the declaration, it thinks it isn&apos;t valid.
<br>
<br>I added some XML parsing calls to my service before the handle() function is called to check for valid XML and avoid the &quot;Bad Request&quot; fault.&#xA0; This also allows me to send back a more suitable error message:
<br>
<br><span class="default">&lt;?php
<br>$parser </span><span class="keyword">= </span><span class="default">xml_parser_create</span><span class="keyword">(</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);
<br>if (!</span><span class="default">xml_parse</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">,</span><span class="default">$HTTP_RAW_POST_DATA</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">)){
<br>&#xA0;&#xA0; </span><span class="default">$webService</span><span class="keyword">-&gt;</span><span class="default">fault</span><span class="keyword">(</span><span class="string">&quot;500&quot;</span><span class="keyword">, </span><span class="string">&quot;Cannot parse XML: &quot;</span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">xml_error_string</span><span class="keyword">(</span><span class="default">xml_get_error_code</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">)).
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot; at line: &quot;</span><span class="keyword">.</span><span class="default">xml_get_current_line_number</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">).
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;, column: &quot;</span><span class="keyword">.</span><span class="default">xml_get_current_column_number</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">));
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soapserver.handle.php)

**[To root](/README.md)**