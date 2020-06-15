# SimpleXMLElement::attributes




<div class="phpcode"><span class="html">
It is really simple to access attributes using array form. However, you must convert them to strings or ints if you plan on passing the values to functions.<br><br><span class="default">&lt;?php<br>SimpleXMLElement Object<br></span><span class="keyword">(<br>&#xA0; &#xA0; [@</span><span class="default">attributes</span><span class="keyword">] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">id</span><span class="keyword">] =&gt; </span><span class="default">55555<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br><br>&#xA0; &#xA0; [</span><span class="default">text</span><span class="keyword">] =&gt; </span><span class="string">&quot;hello world&quot;<br></span><span class="keyword">)<br></span><span class="default">?&gt;<br></span><br>Then using a function<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">xml_attribute</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">, </span><span class="default">$attribute</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if(isset(</span><span class="default">$object</span><span class="keyword">[</span><span class="default">$attribute</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; return (string) </span><span class="default">$object</span><span class="keyword">[</span><span class="default">$attribute</span><span class="keyword">];<br>}<br></span><span class="default">?&gt;<br></span><br>I can get the &quot;id&quot; like this<br><br><span class="default">&lt;?php<br></span><span class="keyword">print </span><span class="default">xml_attribute</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">, </span><span class="string">&apos;id&apos;</span><span class="keyword">); </span><span class="comment">//prints &quot;55555&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that you must provide the namespace if you want to access an attribute of a non-default namespace:<br><br>Consider the following example:<br><br><span class="default">&lt;?php<br>$xml </span><span class="keyword">= &lt;&lt;&lt;XML<br></span><span class="string">&lt;?xml version=&quot;1.0&quot;?&gt;<br>&lt;Workbook xmlns=&quot;urn:schemas-microsoft-com:office:spreadsheet&quot;<br> xmlns:ss=&quot;urn:schemas-microsoft-com:office:spreadsheet&quot;&gt;<br> &lt;Table Foo=&quot;Bar&quot; ss:ExpandedColumnCount=&quot;7&quot;&gt;<br> &lt;/Table&gt;<br>&lt;/Workbook&gt;<br></span><span class="keyword">XML;<br><br></span><span class="default">$sxml </span><span class="keyword">= new </span><span class="default">SimpleXMLElement</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);<br><br></span><span class="comment">/**<br> * Access attribute of default namespace<br> */<br></span><span class="default">var_dump</span><span class="keyword">((string) </span><span class="default">$sxml</span><span class="keyword">-&gt;</span><span class="default">Table</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="string">&apos;Foo&apos;</span><span class="keyword">]);<br></span><span class="comment">// outputs: &apos;Bar&apos;<br><br>/**<br> * Access attribute of non-default namespace<br> */<br></span><span class="default">var_dump</span><span class="keyword">((int) </span><span class="default">$sxml</span><span class="keyword">-&gt;</span><span class="default">Table</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="string">&apos;ExpandedColumnCount&apos;</span><span class="keyword">]);<br></span><span class="comment">// outputs: 0<br><br></span><span class="default">var_dump</span><span class="keyword">((int) </span><span class="default">$sxml</span><span class="keyword">-&gt;</span><span class="default">Table</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]-&gt;</span><span class="default">attributes</span><span class="keyword">(</span><span class="string">&apos;ss&apos;</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">)-&gt;</span><span class="default">ExpandedColumnCount</span><span class="keyword">);<br></span><span class="comment">// outputs: &apos;7&apos;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br>$att </span><span class="keyword">= </span><span class="string">&apos;attribueName&apos;</span><span class="keyword">;<br><br></span><span class="comment">// You can access an element&apos;s attribute just like this :<br></span><span class="default">$attribute </span><span class="keyword">= </span><span class="default">$element</span><span class="keyword">-&gt;</span><span class="default">attributes</span><span class="keyword">()-&gt;</span><span class="default">$att</span><span class="keyword">;<br><br></span><span class="comment">// This will save the value of the attribute, and not the objet<br></span><span class="default">$attribute </span><span class="keyword">= (string)</span><span class="default">$element</span><span class="keyword">-&gt;</span><span class="default">attributes</span><span class="keyword">()-&gt;</span><span class="default">$att</span><span class="keyword">;<br><br></span><span class="comment">// You also can edit it this way :<br></span><span class="default">$element</span><span class="keyword">-&gt;</span><span class="default">attributes</span><span class="keyword">()-&gt;</span><span class="default">$att </span><span class="keyword">= </span><span class="string">&apos;New value of the attribute&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.attributes.php)

**[To root](/README.md)**