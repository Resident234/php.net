# SimpleXMLElement::children




<div class="phpcode"><span class="html">
Here&apos;s a simple, recursive, function to transform XML data into pseudo E4X syntax ie. root.child.value = foobar
<br>
<br><span class="default">&lt;?php
<br>error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);
<br>
<br></span><span class="default">$xml </span><span class="keyword">= new </span><span class="default">SimpleXMLElement</span><span class="keyword">(
<br></span><span class="string">&apos;&lt;Patriarch&gt;
<br>&#xA0;&#xA0; &lt;name&gt;Bill&lt;/name&gt;
<br>&#xA0;&#xA0; &lt;wife&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;name&gt;Vi&lt;/name&gt;
<br>&#xA0;&#xA0; &lt;/wife&gt;
<br>&#xA0;&#xA0; &lt;son&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;name&gt;Bill&lt;/name&gt;
<br>&#xA0;&#xA0; &lt;/son&gt;
<br>&#xA0;&#xA0; &lt;daughter&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;name&gt;Jeri&lt;/name&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;husband&gt;
<br>&#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Mark&lt;/name&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;/husband&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;son&gt;
<br>&#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Greg&lt;/name&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;/son&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;son&gt;
<br>&#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Tim&lt;/name&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;/son&gt;&#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0;&#xA0; &lt;son&gt;
<br>&#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Mark&lt;/name&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;/son&gt;&#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0;&#xA0; &lt;son&gt;
<br>&#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Josh&lt;/name&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;wife&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Kristine&lt;/name&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;/wife&gt; 
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;son&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Blake&lt;/name&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;/son&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;daughter&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;name&gt;Liah&lt;/name&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &lt;/daughter&gt;
<br>&#xA0; &#xA0;&#xA0; &lt;/son&gt;
<br>&#xA0;&#xA0; &lt;/daughter&gt;
<br>&lt;/Patriarch&gt;&apos;</span><span class="keyword">);
<br>
<br></span><span class="default">RecurseXML</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">);
<br>
<br>function </span><span class="default">RecurseXML</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">,</span><span class="default">$parent</span><span class="keyword">=</span><span class="string">&quot;&quot;</span><span class="keyword">)
<br>{
<br>&#xA0;&#xA0; </span><span class="default">$child_count </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0;&#xA0; foreach(</span><span class="default">$xml </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">)
<br>&#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$child_count</span><span class="keyword">++;&#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; if(</span><span class="default">RecurseXML</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">,</span><span class="default">$parent</span><span class="keyword">.</span><span class="string">&quot;.&quot;</span><span class="keyword">.</span><span class="default">$key</span><span class="keyword">) == </span><span class="default">0</span><span class="keyword">)&#xA0; </span><span class="comment">// no childern, aka &quot;leaf node&quot;
<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; print(</span><span class="default">$parent </span><span class="keyword">. </span><span class="string">&quot;.&quot; </span><span class="keyword">. (string)</span><span class="default">$key </span><span class="keyword">. </span><span class="string">&quot; = &quot; </span><span class="keyword">. (string)</span><span class="default">$value </span><span class="keyword">. </span><span class="string">&quot;&lt;BR&gt;\n&quot;</span><span class="keyword">);&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; }&#xA0; &#xA0;&#xA0; 
<br>&#xA0;&#xA0; }
<br>&#xA0;&#xA0; return </span><span class="default">$child_count</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>The output....
<br>
<br>.name = Bill
<br>.wife.name = Vi
<br>.son.name = Bill
<br>.daughter.name = Jeri
<br>.daughter.husband.name = Mark
<br>.daughter.son.name = Greg
<br>.daughter.son.name = Tim
<br>.daughter.son.name = Mark
<br>.daughter.son.name = Josh
<br>.daughter.son.wife.name = Kristine
<br>.daughter.son.son.name = Blake
<br>.daughter.son.daughter.name = Liah</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.children.php)

**[To root](/README.md)**