# simplexml_load_string




<div class="phpcode"><span class="html">
I had a hard time finding this documented, so posting it here in case it helps someone:<br><br>If you want to use multiple libxml options, separate them with a pipe, like so:<br><br><span class="default">&lt;?php<br>$xml </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="string">&apos;SimpleXMLElement&apos;</span><span class="keyword">, </span><span class="default">LIBXML_NOCDATA </span><span class="keyword">| </span><span class="default">LIBXML_NOBLANKS</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A simpler way to transform the result into an array (requires json module).
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">object2array</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">) { return @</span><span class="default">json_decode</span><span class="keyword">(@</span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">),</span><span class="default">1</span><span class="keyword">); }
<br></span><span class="default">?&gt;
<br></span>
<br>Example:
<br><span class="default">&lt;?php
<br>$xml_object</span><span class="keyword">=</span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="string">&apos;&lt;SOME XML DATA&apos;</span><span class="keyword">);
<br></span><span class="default">$xml_array</span><span class="keyword">=</span><span class="default">object2array</span><span class="keyword">(</span><span class="default">$xml_object</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There seems to be a lot of talk about SimpleXML having a &quot;problem&quot; with CDATA, and writing functions to rip it out, etc. I thought so too, at first, but it&apos;s actually behaving just fine under PHP 5.2.6
<br>
<br>The key is noted above example #6 here:
<br><a href="http://uk2.php.net/manual/en/simplexml.examples.php" rel="nofollow" target="_blank">http://uk2.php.net/manual/en/simplexml.examples.php</a>
<br>
<br>&quot;To compare an element or attribute with a string or pass it into a function that requires a string, you must cast it to a string using (string). Otherwise, PHP treats the element as an object.&quot;
<br>
<br>If a tag contains CDATA, SimpleXML remembers that fact, by representing it separately from the string content of the element. So some functions, including print_r(), might not show what you expect. But if you explicitly cast to a string, you get the whole content.
<br>
<br><span class="default">&lt;?php
<br>$xml </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="string">&apos;&lt;foo&gt;Text1 &amp;amp; XML entities&lt;/foo&gt;&apos;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);
<br></span><span class="comment">/*
<br>SimpleXMLElement Object
<br>(
<br>&#xA0; &#xA0; [0] =&gt; Text1 &amp; XML entities
<br>)
<br>*/
<br>
<br></span><span class="default">$xml2 </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="string">&apos;&lt;foo&gt;&lt;![CDATA[Text2 &amp; raw data]]&gt;&lt;/foo&gt;&apos;</span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$xml2</span><span class="keyword">);
<br></span><span class="comment">/*
<br>SimpleXMLElement Object
<br>(
<br>)
<br>*/
<br>// Where&apos;s my CDATA?
<br>
<br>// Let&apos;s try explicit casts
<br></span><span class="default">print_r</span><span class="keyword">( (string)</span><span class="default">$xml </span><span class="keyword">);
<br></span><span class="default">print_r</span><span class="keyword">( (string)</span><span class="default">$xml2 </span><span class="keyword">);
<br></span><span class="comment">/*
<br>Text1 &amp; XML entities
<br>Text2 &amp; raw data
<br>*/
<br>// Much better
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.simplexml-load-string.php)

**[⬆ to root](/)**