# DOMNode::C14N




<div class="phpcode"><span class="html">
C14N() returns an empty string if the node is not included in the document tree:<br><span class="default">&lt;?php<br>$d </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">(</span><span class="string">&apos;1.0&apos;</span><span class="keyword">);<br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">loadXML</span><span class="keyword">(</span><span class="string">&apos;&lt;foo&gt;&lt;/foo&gt;&apos;</span><span class="keyword">);<br></span><span class="default">$n </span><span class="keyword">= </span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">createElement</span><span class="keyword">(</span><span class="string">&apos;bar&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">-&gt;</span><span class="default">C14N</span><span class="keyword">());<br></span><span class="default">$d</span><span class="keyword">-&gt;</span><span class="default">documentElement</span><span class="keyword">-&gt;</span><span class="default">appendChild</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">-&gt;</span><span class="default">C14N</span><span class="keyword">());<br></span><span class="default">?&gt;<br></span>output:<br>string(0) &quot;&quot;<br>string(11) &quot;&lt;bar&gt;&lt;/bar&gt;&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
When working with (malformed) HTML, you&apos;re probably better off using DOMDocument&apos;s saveHTML() method instead. C14N() will attempt to make your HTML valid XML, for example by converting &lt;br&gt; to &lt;br&gt;&lt;/br&gt;.<br><br>So instead of:<br>$html = $Node-&gt;C14N();<br><br>Use:<br>$html = $Node-&gt;ownerDocument-&gt;saveHTML( $Node );</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domnode.c14n.php)

**[⬆ to root](/)**