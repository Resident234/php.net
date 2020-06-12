# SimpleXML




<div class="phpcode"><span class="html">
Three line xml2array:<br><br><span class="default">&lt;?php<br><br>$xml </span><span class="keyword">= </span><span class="default">simplexml_load_string</span><span class="keyword">(</span><span class="default">$xmlstring</span><span class="keyword">);<br></span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);<br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Ta da!</span>
</div>
  

#


<div class="phpcode"><span class="html">
The BIGGEST differece between an XML and a PHP array is that in an XML file, the name of elements can be the same even if they are siblings, eg. &quot;&lt;pa&gt;&lt;ch /&gt;&lt;ch /&gt;&lt;ch /&gt;&lt;/pa&gt;&quot;, while in an PHP array, the key of which must be different.
<br>
<br>I think the array structure developed by svdmeer can fit for XML, and fits well.
<br>
<br>here is an example array converted from an xml file:
<br>array(
<br>&quot;@tag&quot;=&gt;&quot;name&quot;,
<br>&quot;@attr&quot;=&gt;array(
<br>&#xA0; &#xA0; &quot;id&quot;=&gt;&quot;1&quot;,&quot;class&quot;=&gt;&quot;2&quot;)
<br>&quot;@text&quot;=&gt;&quot;some text&quot;,
<br>)
<br>
<br>or if it has childrens, that can be:
<br>
<br>array(
<br>&quot;@tag&quot;=&gt;&quot;name&quot;,
<br>&quot;@attr&quot;=&gt;array(
<br>&#xA0; &#xA0; &quot;id&quot;=&gt;&quot;1&quot;,&quot;class&quot;=&gt;&quot;2&quot;)
<br>&quot;@items&quot;=&gt;array(
<br>&#xA0; &#xA0; 0=&gt;array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &quot;@tag&quot;=&gt;&quot;name&quot;,&quot;@text&quot;=&gt;&quot;some text&quot;
<br>&#xA0; &#xA0; )
<br>)
<br>
<br>Also, I wrote a function that can change that array back to XML.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">array2XML</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">,</span><span class="default">$root</span><span class="keyword">) {
<br></span><span class="default">$xml </span><span class="keyword">= new </span><span class="default">SimpleXMLElement</span><span class="keyword">(</span><span class="string">&quot;&lt;?xml version=\&quot;1.0\&quot; encoding=\&quot;utf-8\&quot; ?&gt;&lt;</span><span class="keyword">{</span><span class="default">$root</span><span class="keyword">}</span><span class="string">&gt;&lt;/</span><span class="keyword">{</span><span class="default">$root</span><span class="keyword">}</span><span class="string">&gt;&quot;</span><span class="keyword">); 
<br></span><span class="default">$f </span><span class="keyword">= </span><span class="default">create_function</span><span class="keyword">(</span><span class="string">&apos;$f,$c,$a&apos;</span><span class="keyword">,</span><span class="string">&apos; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach($a as $v) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset($v[&quot;@text&quot;])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ch = $c-&gt;addChild($v[&quot;@tag&quot;],$v[&quot;@text&quot;]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ch = $c-&gt;addChild($v[&quot;@tag&quot;]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset($v[&quot;@items&quot;])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $f($f,$ch,$v[&quot;@items&quot;]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset($v[&quot;@attr&quot;])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach($v[&quot;@attr&quot;] as $attr =&gt; $val) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ch-&gt;addAttribute($attr,$val);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }&apos;</span><span class="keyword">);
<br></span><span class="default">$f</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">,</span><span class="default">$xml</span><span class="keyword">,</span><span class="default">$arr</span><span class="keyword">);
<br>return </span><span class="default">$xml</span><span class="keyword">-&gt;</span><span class="default">asXML</span><span class="keyword">();
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In reply to&#xA0; soloman at textgrid dot com,<br><br>2 line XML2Array:<br><br>$xml = simplexml_load_string($file);<br>$array = (array)$xml;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.simplexml.php)

**[⬆ to root](/)**