# SimpleXMLElement::addChild




<div class="phpcode"><span class="html">
Here is a class with more functions for SimpleXMLElement :
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> *
<br> * Extension for SimpleXMLElement
<br> * @author Alexandre FERAUD
<br> *
<br> */
<br></span><span class="keyword">class </span><span class="default">ExSimpleXMLElement </span><span class="keyword">extends </span><span class="default">SimpleXMLElement
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; </span><span class="comment">/**
<br>&#xA0; &#xA0;&#xA0; * Add CDATA text in a node
<br>&#xA0; &#xA0;&#xA0; * @param string $cdata_text The CDATA value&#xA0; to add
<br>&#xA0; &#xA0;&#xA0; */
<br>&#xA0; </span><span class="keyword">private function </span><span class="default">addCData</span><span class="keyword">(</span><span class="default">$cdata_text</span><span class="keyword">)
<br>&#xA0; {
<br>&#xA0;&#xA0; </span><span class="default">$node</span><span class="keyword">= </span><span class="default">dom_import_simplexml</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">);
<br>&#xA0;&#xA0; </span><span class="default">$no </span><span class="keyword">= </span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">ownerDocument</span><span class="keyword">;
<br>&#xA0;&#xA0; </span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">appendChild</span><span class="keyword">(</span><span class="default">$no</span><span class="keyword">-&gt;</span><span class="default">createCDATASection</span><span class="keyword">(</span><span class="default">$cdata_text</span><span class="keyword">));
<br>&#xA0; }
<br>
<br>&#xA0; </span><span class="comment">/**
<br>&#xA0;&#xA0; * Create a child with CDATA value
<br>&#xA0;&#xA0; * @param string $name The name of the child element to add.
<br>&#xA0;&#xA0; * @param string $cdata_text The CDATA value of the child element.
<br>&#xA0;&#xA0; */
<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">addChildCData</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">,</span><span class="default">$cdata_text</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$child </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">addChild</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$child</span><span class="keyword">-&gt;</span><span class="default">addCData</span><span class="keyword">(</span><span class="default">$cdata_text</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="comment">/**
<br>&#xA0; &#xA0;&#xA0; * Add SimpleXMLElement code into a SimpleXMLElement
<br>&#xA0; &#xA0;&#xA0; * @param SimpleXMLElement $append
<br>&#xA0; &#xA0;&#xA0; */
<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">appendXML</span><span class="keyword">(</span><span class="default">$append</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$append</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">((string) </span><span class="default">$append</span><span class="keyword">))==</span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$xml </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">addChild</span><span class="keyword">(</span><span class="default">$append</span><span class="keyword">-&gt;</span><span class="default">getName</span><span class="keyword">());
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$append</span><span class="keyword">-&gt;</span><span class="default">children</span><span class="keyword">() as </span><span class="default">$child</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">appendXML</span><span class="keyword">(</span><span class="default">$child</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$xml </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">addChild</span><span class="keyword">(</span><span class="default">$append</span><span class="keyword">-&gt;</span><span class="default">getName</span><span class="keyword">(), (string) </span><span class="default">$append</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$append</span><span class="keyword">-&gt;</span><span class="default">attributes</span><span class="keyword">() as </span><span class="default">$n </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">addAttribute</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">, </span><span class="default">$v</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To complete Volker Grabsch&apos;s comment, stating :<br>&quot;Note that although addChild() escapes &quot;&lt;&quot; and &quot;&gt;&quot;, it does not escape the ampersand &quot;&amp;&quot;.&quot;<br><br>To work around that problem, you can use direct property assignment such as :<br><br><span class="default">&lt;?php<br>$xmlelement</span><span class="keyword">-&gt;</span><span class="default">value </span><span class="keyword">= </span><span class="string">&apos;my value &lt; &gt; &amp;&apos;</span><span class="keyword">;<br></span><span class="comment">// results in &lt;value&gt;my value &amp;lt; &amp;gt; &amp;amp;&lt;/value&gt;<br></span><span class="default">?&gt;<br></span><br>instead of doing :<br><br><span class="default">&lt;?php<br>$xmlelement</span><span class="keyword">-&gt;</span><span class="default">addChild</span><span class="keyword">(</span><span class="string">&apos;value&apos;</span><span class="keyword">, </span><span class="string">&apos;my value &lt; &gt; &amp;&apos;</span><span class="keyword">);<br></span><span class="comment">// results in &lt;value&gt;my value &amp;lt; &amp;gt; &amp;&lt;/value&gt; (invalid XML)<br></span><span class="default">?&gt;<br></span><br>See also: <a href="http://stackoverflow.com/questions/552957" rel="nofollow" target="_blank">http://stackoverflow.com/questions/552957</a> (Rationale behind SimpleXMLElement&apos;s handling of text values in addChild and addAttribute)<br><br>HTH</span>
</div>
  

#


<div class="phpcode"><span class="html">
In the docs for google sitemaps it is required an element for mobile sitemaps that looks like this: &lt;mobile:mobile/&gt;<br><br>I used some time to figure out how to make it, but it is quite simple when understood.<br><br>$mobile_schema = &apos;<a href="http://www.google.com/schemas/sitemap-mobile/1.0" rel="nofollow" target="_blank">http://www.google.com/schemas/sitemap-mobile/1.0</a>&apos;;<br><br>//Create root element<br>$xml_mobile = new SimpleXMLElement(&apos;<br>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;<br>&lt;urlset xmlns=&quot;<a href="http://www.sitemaps.org/schemas/sitemap/0.9" rel="nofollow" target="_blank">http://www.sitemaps.org/schemas/sitemap/0.9</a>&quot; xmlns:mobile=&quot;&apos;.$mobile_schema.&apos;&quot;&gt;&lt;/urlset&gt;<br>&apos;);<br><br>//Add required children<br>$url_mobile = $xml_b_list_mobile-&gt;addChild(&apos;url&apos;);<br>$url_mobile-&gt;addChild(&apos;loc&apos;, &apos;your-mobile-site-url&apos;);<br>$url_mobile-&gt;addChild(&apos;mobile:mobile&apos;, null, $mobile_schema);<br><br>For this to work properly the attribute xmlns:mobile must be set in the root node, and then used as namespace(third argument) when creating the mobile:mobile child with null as value.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.addchild.php)

**[⬆ to root](/)**