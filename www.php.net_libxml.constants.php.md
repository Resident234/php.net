# Predefined Constants




<div class="phpcode"><span class="html">
When inserting XML DOM Elements inside existing XML DOM Elements that I loaded from an XML file using the following code, none of my new elements were formatted correctly, they just showed up on one line:<br><br><span class="default">&lt;?php <br>$dom </span><span class="keyword">= </span><span class="default">DOMDocument</span><span class="keyword">::</span><span class="default">load</span><span class="keyword">(</span><span class="string">&apos;file.xml&apos;</span><span class="keyword">); <br></span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">formatOutput </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br></span><span class="comment">//$dom-&gt;add some new elements with child nodes somewhere inside the loaded XML using insertBefore();<br></span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">saveXML</span><span class="keyword">();<br></span><span class="comment">//output: everything looks normal but the new nodes are all on one line.<br></span><span class="default">?&gt;<br></span><br>I found I could pass LIBXML_NOBLANKS to the load method and it would reformat the whole document, including my added stuff:<br><span class="default">&lt;?php <br>$dom </span><span class="keyword">= </span><span class="default">DOMDocument</span><span class="keyword">::</span><span class="default">load</span><span class="keyword">(</span><span class="string">&apos;file.xml&apos;</span><span class="keyword">, </span><span class="default">LIBXML_NOBLANKS</span><span class="keyword">); <br></span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">formatOutput </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br></span><span class="comment">//$dom-&gt;add some new elements with child nodes somewhere inside the loaded XML using insertBefore();<br></span><span class="default">$dom</span><span class="keyword">-&gt;</span><span class="default">saveXML</span><span class="keyword">();<br></span><span class="comment">//output: everything looks newly formatted, including new nodes<br></span><span class="default">?&gt;<br></span><br>Hope this helps, took me hours of trial and error to figure this out!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/libxml.constants.php)

**[To root](/README.md)**