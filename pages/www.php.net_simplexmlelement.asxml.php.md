# SimpleXMLElement::asXML




<div class="phpcode"><span class="html">
To prevent asXML from encoding vowels unwantedly, simply use an approriate XML header with encoding in advance.<br><br>If you do so, asXML will happily leave your vowels (and the header) entirely untouched.<br><br><span class="default">&lt;?php<br><br>$xmlstr </span><span class="keyword">=<br></span><span class="string">&apos;&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;<br>&lt;keys&gt;<br>&#xA0; &lt;key lang=&quot;en&quot;&gt;&amp;lt;Insert&amp;gt;&lt;/key&gt;<br>&#xA0; &lt;key lang=&quot;de&quot;&gt;&amp;lt;Einf&#xFC;gen&amp;gt;&lt;/key&gt;<br>&lt;/keys&gt;&apos;</span><span class="keyword">;<br><br></span><span class="default">$sxe </span><span class="keyword">= new </span><span class="default">SimpleXMLElement</span><span class="keyword">(</span><span class="default">$xmlstr</span><span class="keyword">);<br><br></span><span class="default">$output </span><span class="keyword">= </span><span class="default">$sxe</span><span class="keyword">-&gt;</span><span class="default">asXML</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>$xmlstr and $output are identical now.<br><br>The subsequent use of html_entity_decode() (as proposed in the very beginning in another post) has several drawbacks:<br><br>1. It is slow<br>2. It is expensive<br>3. If there are already encoded arrow brackets or double quotes in your source for instance (as shown in the above example), markup will be broken.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.asxml.php)

**[To root](/README.md)**