# Examples




<div class="phpcode"><span class="html">
Here&apos;s a very simple example on how to use PHP5 to transform a XML file using a XSL file.<br><br><span class="default">&lt;?php<br><br>&#xA0;&#xA0; $xslDoc </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">();<br>&#xA0;&#xA0; </span><span class="default">$xslDoc</span><span class="keyword">-&gt;</span><span class="default">load</span><span class="keyword">(</span><span class="string">&quot;collection.xsl&quot;</span><span class="keyword">);<br><br>&#xA0;&#xA0; </span><span class="default">$xmlDoc </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">();<br>&#xA0;&#xA0; </span><span class="default">$xmlDoc</span><span class="keyword">-&gt;</span><span class="default">load</span><span class="keyword">(</span><span class="string">&quot;collection.xml&quot;</span><span class="keyword">);<br><br>&#xA0;&#xA0; </span><span class="default">$proc </span><span class="keyword">= new </span><span class="default">XSLTProcessor</span><span class="keyword">();<br>&#xA0;&#xA0; </span><span class="default">$proc</span><span class="keyword">-&gt;</span><span class="default">importStylesheet</span><span class="keyword">(</span><span class="default">$xslDoc</span><span class="keyword">);<br>&#xA0;&#xA0; echo </span><span class="default">$proc</span><span class="keyword">-&gt;</span><span class="default">transformToXML</span><span class="keyword">(</span><span class="default">$xmlDoc</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>For the sake of simplicity there&apos;s no error handling on this code. I hope this helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/xsl.examples.php)

**[⬆ to root](/)**