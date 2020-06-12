# simplexml_load_file




<div class="phpcode"><span class="html">
Sometimes we have xml&apos;s with hyphens nodes, like<br><br>&lt;my_xml&gt;<br> &lt;some-node&gt;value&lt;/some-node&gt;<br>&lt;/my_xml&gt;<br><br>You&apos;ll need to use<br><span class="default">&lt;?php<br>$simpleXmlObj</span><span class="keyword">-&gt;{</span><span class="string">&apos;some-node&apos;</span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>instead of <br><span class="default">&lt;?php<br>$simpleXmlObj</span><span class="keyword">-&gt;</span><span class="default">some</span><span class="keyword">-</span><span class="default">node</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To correctly extract a value from a CDATA just make sure you cast the SimpleXML Element to a string value by using the cast operator:
<br>
<br><span class="default">&lt;?php
<br>$xml </span><span class="keyword">= </span><span class="string">&apos;&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; ?&gt;
<br>&lt;rss&gt;
<br>&#xA0; &#xA0; &lt;channel&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &lt;item&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;title&gt;&lt;![CDATA[Tom &amp; Jerry]]&gt;&lt;/title&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &lt;/item&gt;
<br>&#xA0; &#xA0; &lt;/channel&gt;
<br>&lt;/rss&gt;&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">$xml </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);
<br>
<br></span><span class="comment">// echo does the casting for you
<br></span><span class="keyword">echo </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">channel</span><span class="keyword">-&gt;</span><span class="default">item</span><span class="keyword">-&gt;</span><span class="default">title</span><span class="keyword">;
<br>
<br></span><span class="comment">// but vardump (or print_r) not!
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">channel</span><span class="keyword">-&gt;</span><span class="default">item</span><span class="keyword">-&gt;</span><span class="default">title</span><span class="keyword">);
<br>
<br></span><span class="comment">// so cast the SimpleXML Element to &apos;string&apos; solve this issue
<br></span><span class="default">var_dump</span><span class="keyword">((string) </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">channel</span><span class="keyword">-&gt;</span><span class="default">item</span><span class="keyword">-&gt;</span><span class="default">title</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Above will output:
<br>
<br>Tom &amp; Jerry
<br>
<br>object(SimpleXMLElement)#4 (0) {}
<br>
<br>string(11) &quot;Tom &amp; Jerry&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.simplexml-load-file.php)

**[⬆ to root](/)**