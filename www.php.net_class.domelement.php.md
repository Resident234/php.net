# The DOMElement class




<div class="phpcode"><span class="html">
Caveat!<br>It took me almost an hour to figure this out, so I hope it saves at least one of you some time.<br><br>If you want to debug your DOM tree and try var_dump() or similar you will be fooled into thinking the DOMElement that you are looking at is empty, because var_dump() says: object(DOMElement)#1 (0) { } <br><br>After much debugging I found out that all DOM objects are invisible to var_dump() and print_r(), my guess is because they are C objects and not PHP objects. So I tried saveXML(), which works fine on DOMDocument, but is not implemented on DOMElement.<br><br>The solution is simple (if you know it):<br>$xml = $domElement-&gt;ownerDocument-&gt;saveXML($domElement);<br><br>This will give you an XML representation of $domElement.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi to get the value of DOMElement just get the nodeValue public parameter (it is inherited from DOMNode):<br><span class="default">&lt;?php <br></span><span class="keyword">echo </span><span class="default">$domElement</span><span class="keyword">-&gt;</span><span class="default">nodeValue</span><span class="keyword">; <br></span><span class="default">?&gt;<br></span>Everything is obvious if you now about this thing ;-)</span>
</div>
  

#


<div class="phpcode"><span class="html">
This page doesn&apos;t list the inherited properties from DOMNode, e.g. the quite important textContent property. It would be immensely helpful if it would list those as well.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi!
<br>
<br>Combining all th comments, the easiest way to get inner HTML of the node is to use this function:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">get_inner_html</span><span class="keyword">( </span><span class="default">$node </span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$innerHTML</span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$children </span><span class="keyword">= </span><span class="default">$node</span><span class="keyword">-&gt;</span><span class="default">childNodes</span><span class="keyword">;
<br>&#xA0; &#xA0; foreach (</span><span class="default">$children </span><span class="keyword">as </span><span class="default">$child</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$innerHTML </span><span class="keyword">.= </span><span class="default">$child</span><span class="keyword">-&gt;</span><span class="default">ownerDocument</span><span class="keyword">-&gt;</span><span class="default">saveXML</span><span class="keyword">( </span><span class="default">$child </span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$innerHTML</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.domelement.php)

**[⬆ to root](/)**