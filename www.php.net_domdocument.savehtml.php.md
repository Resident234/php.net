# DOMDocument::saveHTML




<div class="phpcode"><span class="html">
As of PHP 5.4 and Libxml 2.6, there is currently simpler approach:<br><br>when you load html as this<br><br>$html-&gt;loadHTML($content, LIBXML_HTML_NOIMPLIED | LIBXML_HTML_NODEFDTD);<br><br>in the output, there will be no doctype, html or body tags</span>
</div>
  

#


<div class="phpcode"><span class="html">
When saving HTML fragment initiated with LIBXML_HTML_NOIMPLIED option, it will end up being &quot;broken&quot; as libxml requires root element. libxml will attempt to fix the fragment by adding closing tag at the end of string based on the first opened tag it encounters in the fragment. <br><br>For an example:<br><br>&lt;h1&gt;Foo&lt;/h1&gt;&lt;p&gt;bar&lt;/p&gt;<br><br>will end up as:<br><br>&lt;h1&gt;Foo&lt;p&gt;bar&lt;/p&gt;&lt;/h1&gt;<br><br>Easiest workaround is adding root tag yourself and stripping it later:<br><br>$html-&gt;loadHTML(&apos;&lt;html&gt;&apos; . $content .&apos;&lt;/html&gt;&apos;, LIBXML_HTML_NOIMPLIED | LIBXML_HTML_NODEFDTD);<br><br>$content = str_replace(array(&apos;&lt;html&gt;&apos;,&apos;&lt;/html&gt;&apos;) , &apos;&apos; , $html-&gt;saveHTML());</span>
</div>
  

#


<div class="phpcode"><span class="html">
I am using this solution to prevent tags and the doctype from being added to the HTML string automatically:<br><br><span class="default">&lt;?php<br>$html </span><span class="keyword">= </span><span class="string">&apos;&lt;h1&gt;Hello world!&lt;/h1&gt;&apos;</span><span class="keyword">;<br></span><span class="default">$html </span><span class="keyword">= </span><span class="string">&apos;&lt;div&gt;&apos; </span><span class="keyword">. </span><span class="default">$html </span><span class="keyword">. </span><span class="string">&apos;&lt;/div&gt;&apos;</span><span class="keyword">;<br></span><span class="default">$doc </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">;<br></span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">loadHTML</span><span class="keyword">(</span><span class="default">$html</span><span class="keyword">);<br>echo </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">saveXML</span><span class="keyword">(</span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">getElementsByTagName</span><span class="keyword">(</span><span class="string">&apos;div&apos;</span><span class="keyword">)-&gt;</span><span class="default">item</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">)), </span><span class="default">5</span><span class="keyword">, -</span><span class="default">6</span><span class="keyword">)<br><br></span><span class="comment">// Outputs: &quot;&lt;h1&gt;Hello world!&lt;/h1&gt;&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This method, as of 5.2.6, will automatically add &lt;html&gt;&lt;body&gt; and &lt;!DOCTYPE&gt; tags to the document if they are missing, without asking whether you want them. In my application, I needed to use the DOM methods to manipulate just a fragment of html, so these tags were rather unhelpful.<br><br>Here&apos;s a simple hack to remove them in case, like me, all you wanted to do was perform a few operations on an HTML fragment.<br><br>$html_fragment = preg_replace(&apos;/^&lt;!DOCTYPE.+?&gt;/&apos;, &apos;&apos;, str_replace( array(&apos;&lt;html&gt;&apos;, &apos;&lt;/html&gt;&apos;, &apos;&lt;body&gt;&apos;, &apos;&lt;/body&gt;&apos;), array(&apos;&apos;, &apos;&apos;, &apos;&apos;, &apos;&apos;), $dom-&gt;saveHTML()));</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.savehtml.php)

**[To root](/README.md)**