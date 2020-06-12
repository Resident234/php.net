# SimpleXMLElement::xpath




<div class="phpcode"><span class="html">
To run an xpath query on an XML document that has a namespace, the namespace must be registered with SimpleXMLElement::registerXPathNamespace() before running the query. If the XML document namespace does not include a prefix, you must make up an arbitrary one, and then use it in your query.<br><br><span class="default">&lt;?php<br>$strXml</span><span class="keyword">= &lt;&lt;&lt;XML<br></span><span class="string">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;<br>&lt;mydoc xmlns=&quot;<a href="http://www.url.com/myns" rel="nofollow" target="_blank">http://www.url.com/myns</a>&quot;&gt;<br>&#xA0; &#xA0; &lt;message&gt;Test message&lt;/message&gt;<br>&lt;/mydoc&gt;<br></span><span class="keyword">XML;<br><br></span><span class="default">$xmlDoc</span><span class="keyword">=new \</span><span class="default">SimpleXMLElement</span><span class="keyword">(</span><span class="default">$strXml</span><span class="keyword">);<br><br>foreach(</span><span class="default">$xmlDoc</span><span class="keyword">-&gt;</span><span class="default">getDocNamespaces</span><span class="keyword">() as </span><span class="default">$strPrefix </span><span class="keyword">=&gt; </span><span class="default">$strNamespace</span><span class="keyword">) {<br>&#xA0; &#xA0; if(</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$strPrefix</span><span class="keyword">)==</span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$strPrefix</span><span class="keyword">=</span><span class="string">&quot;a&quot;</span><span class="keyword">; </span><span class="comment">//Assign an arbitrary namespace prefix.<br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; </span><span class="default">$xmlDoc</span><span class="keyword">-&gt;</span><span class="default">registerXPathNamespace</span><span class="keyword">(</span><span class="default">$strPrefix</span><span class="keyword">,</span><span class="default">$strNamespace</span><span class="keyword">);<br>}<br><br>print(</span><span class="default">$xmlDoc</span><span class="keyword">-&gt;</span><span class="default">xpath</span><span class="keyword">(</span><span class="string">&quot;//a:message&quot;</span><span class="keyword">)[</span><span class="default">0</span><span class="keyword">]); </span><span class="comment">//Use the arbitrary namespace prefix in the query.<br></span><span class="default">?&gt;<br></span><br>This will output:<br><br>Test message</span>
</div>
  

#


<div class="phpcode"><span class="html">
On a xml that have namespace you need to do this before your xpath request (or empty array will be return) :
<br>
<br><span class="default">&lt;?php
<br>$string </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;xmlns=&apos;</span><span class="keyword">, </span><span class="string">&apos;ns=&apos;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">); </span><span class="comment">//$string is a string that contains xml...
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
xpath() can also be used to select elements by their attributes. For a good XPath reference check out: <a href="http://www.w3schools.com/xpath/xpath_syntax.asp" rel="nofollow" target="_blank">http://www.w3schools.com/xpath/xpath_syntax.asp</a>
<br>
<br><span class="default">&lt;?php
<br>$string </span><span class="keyword">= &lt;&lt;&lt;XML
<br></span><span class="string">&lt;sizes&gt;
<br>&#xA0; &#xA0; &lt;size label=&quot;Square&quot; width=&quot;75&quot; height=&quot;75&quot; /&gt;
<br>&#xA0; &#xA0; &lt;size label=&quot;Thumbnail&quot; width=&quot;100&quot; height=&quot;62&quot; /&gt;
<br>&#xA0; &#xA0; &lt;size label=&quot;Small&quot; width=&quot;112&quot; height=&quot;69&quot; /&gt;
<br>&#xA0; &#xA0; &lt;size label=&quot;Large&quot; width=&quot;112&quot; height=&quot;69&quot; /&gt;
<br>&lt;/sizes&gt;
<br></span><span class="keyword">XML;
<br>
<br></span><span class="default">$xml </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);
<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">xpath</span><span class="keyword">(</span><span class="string">&quot;//size[@label=&apos;Large&apos;]&quot;</span><span class="keyword">);
<br>
<br></span><span class="comment">// print the first (and only) member of the array
<br></span><span class="keyword">echo </span><span class="default">$result</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]-&gt;</span><span class="default">asXml</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br>The script would print: 
<br>&lt;size label=&quot;Large&quot; width=&quot;112&quot; height=&quot;69&quot;/&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to find easly all records satisfying some condition in XML data like 
<br>
<br>....
<br>&#xA0;&#xA0; &lt;book id=&quot;bk101&quot;&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;author&gt;Gambardella, Matthew&lt;/author&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;title&gt;XML Developer&apos;s Guide&lt;/title&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;genre&gt;Computer&lt;/genre&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;price&gt;44.95&lt;/price&gt;
<br>&#xA0;&#xA0; &lt;/book&gt;
<br>&#xA0;&#xA0; &lt;book id=&quot;bk102&quot;&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;author&gt;Ralls, Kim&lt;/author&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;title&gt;Midnight Rain&lt;/title&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;genre&gt;Fantasy&lt;/genre&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;price&gt;5.95&lt;/price&gt;
<br>&#xA0;&#xA0; &lt;/book&gt;
<br>...
<br>
<br>try example below
<br>
<br><span class="default">&lt;?php
<br>
<br>$xmlStr </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;data/books.xml&apos;</span><span class="keyword">);
<br></span><span class="default">$xml </span><span class="keyword">= new </span><span class="default">SimpleXMLElement</span><span class="keyword">(</span><span class="default">$xmlStr</span><span class="keyword">);
<br></span><span class="comment">// seach records by tag value:
<br>// find all book records with price higher than 40$
<br></span><span class="default">$res </span><span class="keyword">= </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">xpath</span><span class="keyword">(</span><span class="string">&quot;book/price[.&gt;&apos;40&apos;]/parent::*&quot;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>You will see response like:
<br>Array (
<br>[0] =&gt; SimpleXMLElement Object
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [@attributes] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; bk101
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [author] =&gt; Gambardella, Matthew
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [title] =&gt; XML Developer&apos;s Guide
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [genre] =&gt; Computer
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [price] =&gt; 44.95
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [publish_date] =&gt; 2000-10-01
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [description] =&gt; An in-depth look at creating applications 
<br>&#xA0; &#xA0; &#xA0; with XML.
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.xpath.php)

**[⬆ to root](/)**