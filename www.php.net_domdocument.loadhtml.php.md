# DOMDocument::loadHTML




<div class="phpcode"><span class="html">
You can also load HTML as UTF-8 using this simple hack:<br><br><span class="default">&lt;?php<br><br>$doc </span><span class="keyword">= new </span><span class="default">DOMDocument</span><span class="keyword">();<br></span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">loadHTML</span><span class="keyword">(</span><span class="string">&apos;&lt;?xml encoding=&quot;UTF-8&quot;&gt;&apos; </span><span class="keyword">. </span><span class="default">$html</span><span class="keyword">);<br><br></span><span class="comment">// dirty fix<br></span><span class="keyword">foreach (</span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">childNodes </span><span class="keyword">as </span><span class="default">$item</span><span class="keyword">)<br>&#xA0; &#xA0; if (</span><span class="default">$item</span><span class="keyword">-&gt;</span><span class="default">nodeType </span><span class="keyword">== </span><span class="default">XML_PI_NODE</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">removeChild</span><span class="keyword">(</span><span class="default">$item</span><span class="keyword">); </span><span class="comment">// remove hack<br></span><span class="default">$doc</span><span class="keyword">-&gt;</span><span class="default">encoding </span><span class="keyword">= </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">; </span><span class="comment">// insert proper<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
DOMDocument is very good at dealing with imperfect markup, but it throws warnings all over the place when it does. <br><br>This isn&apos;t well documented here. The solution to this is to implement a separate aparatus for dealing with just these errors. <br><br>Set libxml_use_internal_errors(true) before calling loadHTML. This will prevent errors from bubbling up to your default error handler. And you can then get at them (if you desire) using other libxml error functions. <br><br>You can find more info here <a href="http://www.php.net/manual/en/ref.libxml.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/ref.libxml.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
When using loadHTML() to process UTF-8 pages, you may meet the problem that the output of dom functions are not like the input. For example, if you want to get &quot;C&#x1EA1;nh tranh&quot;, you will receive &quot;C&#xE1;&#xBA;&#xA1;nh tranh&quot;.&#xA0; I suggest we use mb_convert_encoding before load UTF-8 page :
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; $pageDom </span><span class="keyword">= new </span><span class="default">DomDocument</span><span class="keyword">();&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$searchPage </span><span class="keyword">= </span><span class="default">mb_convert_encoding</span><span class="keyword">(</span><span class="default">$htmlUTF8Page</span><span class="keyword">, </span><span class="string">&apos;HTML-ENTITIES&apos;</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">); 
<br>&#xA0; &#xA0; @</span><span class="default">$pageDom</span><span class="keyword">-&gt;</span><span class="default">loadHTML</span><span class="keyword">(</span><span class="default">$searchPage</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Pay attention when loading html that has a different charset than iso-8859-1. Since this method does not actively try to figure out what the html you are trying to load is encoded in (like most browsers do), you have to specify it in the html head. If, for instance, your html is in utf-8, make sure you have a meta tag in the html&apos;s head section:<br><br>&lt;head&gt;<br>&lt;meta http-equiv=&quot;Content-Type&quot; content=&quot;text/html; charset=utf-8&quot;/&gt;<br>&lt;/head&gt;<br><br>If you do not specify the charset like this, all high-ascii bytes will be html-encoded. It is not enough to set the dom document you are loading the html in to UTF-8.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.loadhtml.php)

**[⬆ to root](/)**