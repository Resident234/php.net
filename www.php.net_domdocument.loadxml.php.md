# DOMDocument::loadXML




<div class="phpcode"><span class="html">
Always remember that with the default parameters this function doesn&apos;t handle well large files, i.e. if a text node is longer than 10Mb it can raise an exception stating:<br><br>DOMDocument::loadXML(): internal error Extra content at the end of the document in Entity<br><br>even though the XML is fine.<br><br>The cause is a definition in parserInternals.h of lixml:<br>#define XML_MAX_TEXT_LENGTH 10000000<br><br>To allow the function to process larger files, pass the LIBXML_PARSEHUGE as an option and it will work just fine:<br><br>$domDocument-&gt;loadXML($xml, LIBXML_PARSEHUGE);</span>
</div>
  

#


<div class="phpcode"><span class="html">
loadXml reports an error instead of throwing an exception when the xml is not well formed. This is annoying if you are trying to to loadXml() in a try...catch statement. Apparently its a feature, not a bug, because this conforms to a spefication. <br><br>If you want to catch an exception instead of generating a report, you could do something like<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">HandleXmlError</span><span class="keyword">(</span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">, </span><span class="default">$errfile</span><span class="keyword">, </span><span class="default">$errline</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if (</span><span class="default">$errno</span><span class="keyword">==</span><span class="default">E_WARNING </span><span class="keyword">&amp;&amp; (</span><span class="default">substr_count</span><span class="keyword">(</span><span class="default">$errstr</span><span class="keyword">,</span><span class="string">&quot;DOMDocument::loadXML()&quot;</span><span class="keyword">)&gt;</span><span class="default">0</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">DOMException</span><span class="keyword">(</span><span class="default">$errstr</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>}<br><br>function </span><span class="default">XmlLoader</span><span class="keyword">(</span><span class="default">$strXml</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">set_error_handler</span><span class="keyword">(</span><span class="string">&apos;HandleXmlError&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$dom </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">loadXml</span><span class="keyword">(</span><span class="default">$strXml</span><span class="keyword">);&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">restore_error_handler</span><span class="keyword">();<br>&#xA0; &#xA0; return </span><span class="default">$dom</span><span class="keyword">;<br> }<br><br></span><span class="default">?&gt;<br></span><br>Returning false in function HandleXmlError() causes a fallback to the default error handler.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.loadxml.php)

**[⬆ to root](/)**