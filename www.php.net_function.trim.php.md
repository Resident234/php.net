# trim




<div class="phpcode"><span class="html">
It may be useful to know that trim() returns an empty string when the argument is an unset/null variable.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When specifying the character mask, <br>make sure that you use double quotes<br><br><span class="default">&lt;?php<br>&#xA0; $hello </span><span class="keyword">= </span><span class="string">&quot; <br>&#xA0; &#xA0; &#xA0; Hello World&#xA0;&#xA0; &quot;</span><span class="keyword">; </span><span class="comment">//here is a string with some trailing and leading whitespace<br><br>&#xA0; </span><span class="default">$trimmed_correct&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$hello</span><span class="keyword">, </span><span class="string">&quot; \t\n\r&quot;</span><span class="keyword">); </span><span class="comment">//&lt;--------OKAY<br>&#xA0; </span><span class="default">$trimmed_incorrect </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$hello</span><span class="keyword">, </span><span class="string">&apos; \t\n\r&apos;</span><span class="keyword">); </span><span class="comment">//&lt;--------NOT AS EXPECTED<br><br>&#xA0; </span><span class="keyword">print(</span><span class="string">&quot;----------------------------&quot;</span><span class="keyword">);<br>&#xA0; print(</span><span class="string">&quot;TRIMMED OK:&quot;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">);<br>&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$trimmed_correct</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">);<br>&#xA0; print(</span><span class="string">&quot;----------------------------&quot;</span><span class="keyword">);<br>&#xA0; print(</span><span class="string">&quot;TRIMMING NOT OK:&quot;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">);<br>&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$trimmed_incorrect</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">);<br>&#xA0; print(</span><span class="string">&quot;----------------------------&quot;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Here is the output:<br><br>----------------------------TRIMMED OK:<br>Hello World<br>----------------------------TRIMMING NOT OK:<br><br>&#xA0; &#xA0; &#xA0; Hello World<br>----------------------------</span>
</div>
  

#


<div class="phpcode"><span class="html">
Non-breaking spaces can be troublesome with trim:
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// turn some HTML with non-breaking spaces into a &quot;normal&quot; string
<br></span><span class="default">$myHTML </span><span class="keyword">= </span><span class="string">&quot;&amp;nbsp;abc&quot;</span><span class="keyword">;
<br></span><span class="default">$converted </span><span class="keyword">= </span><span class="default">strtr</span><span class="keyword">(</span><span class="default">$myHTML</span><span class="keyword">, </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">get_html_translation_table</span><span class="keyword">(</span><span class="default">HTML_ENTITIES</span><span class="keyword">, </span><span class="default">ENT_QUOTES</span><span class="keyword">)));
<br>
<br></span><span class="comment">// this WILL NOT work as expected
<br>// $converted will still appear as &quot; abc&quot; in view source
<br>// (but not in od -x)
<br></span><span class="default">$converted </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$converted</span><span class="keyword">);
<br>
<br></span><span class="comment">// &amp;nbsp; are translated to 0xA0, so use:
<br></span><span class="default">$converted </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$converted</span><span class="keyword">, </span><span class="string">&quot;\xA0&quot;</span><span class="keyword">); </span><span class="comment">// &lt;- THIS DOES NOT WORK
<br>
<br>// EDITED&gt;&gt;
<br>// UTF encodes it as chr(0xC2).chr(0xA0)
<br></span><span class="default">$converted </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$converted</span><span class="keyword">,</span><span class="default">chr</span><span class="keyword">(</span><span class="default">0xC2</span><span class="keyword">).</span><span class="default">chr</span><span class="keyword">(</span><span class="default">0xA0</span><span class="keyword">)); </span><span class="comment">// should work
<br>
<br>// PS: Thanks to John for saving my sanity!
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To remove multiple occurences of whitespace characters in a string an convert them all into single spaces, use this:<br><br>&lt;?<br><br>$text = preg_replace(&apos;/\s+/&apos;, &apos; &apos;, $text);<br><br>?&gt;<br><br>------------<br>JUBI<br><a href="http://www.jubi.buum.pl" rel="nofollow" target="_blank">http://www.jubi.buum.pl</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Another way to trim all the elements of an array<br><span class="default">&lt;?php<br>$newarray </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;trim&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.trim.php)

**[⬆ to root](/)**