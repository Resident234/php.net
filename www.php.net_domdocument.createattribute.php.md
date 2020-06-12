# DOMDocument::createAttribute




<div class="phpcode"><span class="html">
Just in case it isn&apos;t clear (like I had), an example:<br><br><span class="default">&lt;?php<br><br>$domDocument </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">(</span><span class="string">&apos;1.0&apos;</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);<br></span><span class="default">$domElement </span><span class="keyword">= </span><span class="default">$domDocument</span><span class="keyword">-&gt;</span><span class="default">createElement</span><span class="keyword">(</span><span class="string">&apos;field&apos;</span><span class="keyword">,</span><span class="string">&apos;some random data&apos;</span><span class="keyword">);<br></span><span class="default">$domAttribute </span><span class="keyword">= </span><span class="default">$domDocument</span><span class="keyword">-&gt;</span><span class="default">createAttribute</span><span class="keyword">(</span><span class="string">&apos;name&apos;</span><span class="keyword">);<br><br></span><span class="comment">// Value for the created attribute<br></span><span class="default">$domAttribute</span><span class="keyword">-&gt;</span><span class="default">value </span><span class="keyword">= </span><span class="string">&apos;attributevalue&apos;</span><span class="keyword">;<br><br></span><span class="comment">// Don&apos;t forget to append it to the element<br></span><span class="default">$domElement</span><span class="keyword">-&gt;</span><span class="default">appendChild</span><span class="keyword">(</span><span class="default">$domAttribute</span><span class="keyword">);<br><br></span><span class="comment">// Append it to the document itself<br></span><span class="default">$domDocument</span><span class="keyword">-&gt;</span><span class="default">appendChild</span><span class="keyword">(</span><span class="default">$domElement</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Will output:<br>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;<br>&lt;field name=&quot;attributevalue&quot;&gt;some random data&lt;/field&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.createattribute.php)

**[⬆ to root](/)**