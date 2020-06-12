# DOMDocument::getElementsByTagName




<div class="phpcode"><span class="html">
Return if there are no matches is an empty DOMNodeList. Check using length property, e.g.:
<br>
<br><span class="default">&lt;?php
<br>$nodes</span><span class="keyword">=</span><span class="default">$domDocument</span><span class="keyword">-&gt;</span><span class="default">getElementsByTagName</span><span class="keyword">(</span><span class="string">&apos;book&apos;</span><span class="keyword">) ; 
<br>if (</span><span class="default">$nodes</span><span class="keyword">-&gt;</span><span class="default">length</span><span class="keyword">==</span><span class="default">0</span><span class="keyword">) {
<br>&#xA0;&#xA0; </span><span class="comment">// no results
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that when using getElementsByTagName that it is a dynamic list. Thus if you have code which adjusts the DOM structure it will change the results of the getElementsByTagName results list.<br><br>The following code iterates through a complete set of results and changes them all to a new tag:<br><br><span class="default">&lt;?php<br> $nodes </span><span class="keyword">= </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">getElementsByTagName </span><span class="keyword">(</span><span class="string">&quot;oldtag&quot;</span><span class="keyword">);<br><br> </span><span class="default">$nodeListLength </span><span class="keyword">= </span><span class="default">$nodes</span><span class="keyword">-&gt;</span><span class="default">length</span><span class="keyword">; </span><span class="comment">// this value will also change<br> </span><span class="keyword">for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$nodeListLength</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">++)<br> {<br>&#xA0; &#xA0; </span><span class="default">$node </span><span class="keyword">= </span><span class="default">$nodes</span><span class="keyword">-&gt;</span><span class="default">item</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// some code to change the tag name from &quot;oldtag&quot; to something else<br>&#xA0; &#xA0; // e.g. encrypting a tag element<br> </span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>Since the list is dynamically updating, $nodes-&gt;item(0) is the next &quot;unchanged&quot; tag.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.getelementsbytagname.php)

**[⬆ to root](/)**