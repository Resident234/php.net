# DOMXPath::query




<div class="phpcode"><span class="html">
If the query() function seems to ignore your $contextnode, and instead returns all the tags in the document, try to use a relative path (use a . in front of the query):<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $xml </span><span class="keyword">= </span><span class="string">&quot;&lt;?xml version=&apos;1.0&apos; encoding=&apos;UTF-8&apos;?&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &lt;test&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;tag1&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;uselesstag&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;tag2&gt;test&lt;/tag2&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;/uselesstag&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;/tag1&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;tag2&gt;test2&lt;/tag2&gt;<br>&#xA0; &#xA0; &#xA0; &#xA0; &lt;/test&gt;&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; <br>&#xA0; &#xA0; </span><span class="default">$dom </span><span class="keyword">= new </span><span class="default">DomDocument</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">loadXML</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$xpath </span><span class="keyword">= new </span><span class="default">DomXPath</span><span class="keyword">(</span><span class="default">$dom</span><span class="keyword">);<br>&#xA0;&#xA0; <br>&#xA0; &#xA0; </span><span class="default">$tag1 </span><span class="keyword">= </span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">getElementsByTagName</span><span class="keyword">(</span><span class="string">&quot;tag1&quot;</span><span class="keyword">)-&gt;</span><span class="default">item</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);<br>&#xA0;&#xA0; <br>&#xA0; &#xA0; echo </span><span class="default">$xpath</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;//tag2&quot;</span><span class="keyword">)-&gt;</span><span class="default">length</span><span class="keyword">; </span><span class="comment">//output 2 -&gt; correct<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">$xpath</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;//tag2&quot;</span><span class="keyword">, </span><span class="default">$tag1</span><span class="keyword">)-&gt;</span><span class="default">length</span><span class="keyword">; </span><span class="comment">//output 2 -&gt; wrong, the query is not relative<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">$xpath</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;.//tag2&quot;</span><span class="keyword">, </span><span class="default">$tag1</span><span class="keyword">)-&gt;</span><span class="default">length</span><span class="keyword">; </span><span class="comment">//output 1 -&gt; correct (note the dot in front of //)<br></span><span class="default">?&gt;<br></span><br>See that i couldn&apos;t use $xpath-&gt;query(&quot;tag2&quot;, $tag1) as per the documentation, since &quot;tag2&quot; is not a direct child of &quot;tag1&quot;.<br>I don&apos;t know why this note was deleted, i just tested it and it&apos;s correct.<br>It&apos;s not a bug, it&apos;s simply not written in the documentation.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that if your DOMDocument was loaded from HTML, where element and attribute names are case-insensitive, the DOM parser converts them all to lower-case, so your XPath queries will have to as well; &apos;//A/@HREF&apos; won&apos;t find anything even if the original HTML contained &quot;&lt;A HREF=&apos;example.com&apos;&gt;&quot;.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that what clochix says is valid for *any* document which has a default namespace (as it is the case for XHTML).<br><br>This document :<br><br>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;<br><br>&lt;root xmlns=&quot;<a href="http://www.exemple.org/namespace" rel="nofollow" target="_blank">http://www.exemple.org/namespace</a>&quot;&gt;<br><br>&#xA0; &#xA0; &lt;element id=&quot;1&quot;&gt;<br>&#xA0; &#xA0; ...<br>&#xA0; &#xA0; &lt;/element&gt;<br><br>&#xA0; &#xA0; &lt;element id=&quot;2&quot;&gt;<br>&#xA0; &#xA0; ...<br>&#xA0; &#xA0; &lt;/element&gt;<br><br>&lt;/element&gt;<br><br>must be accessed this way :<br><br>$document = new DOMDocument();<br>$document-&gt;load(&apos;document.xml&apos;);<br><br>$xpath = new DOMXPath($document);<br>$xpath-&gt;registerNameSpace(&apos;fakeprefix&apos;, &apos;<a href="http://www.exemple.org/namespace" rel="nofollow" target="_blank">http://www.exemple.org/namespace</a>&apos;);<br><br>$elements = $xpath-&gt;query(&apos;//fakeprefix:element&apos;);<br><br>Of course, there is no prefix in the original document, but the DOMXPath class *needs* one, whatever it is, if you use a default namespace. It *doesn&apos;t work* if you specify an empty prefix like this :<br><br>$xpath-&gt;registerNameSpace(&apos;&apos;, &apos;<a href="http://www.exemple.org/namespace" rel="nofollow" target="_blank">http://www.exemple.org/namespace</a>&apos;);<br><br>Hope this help to spare some time...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domxpath.query.php)

**[To root](/README.md)**