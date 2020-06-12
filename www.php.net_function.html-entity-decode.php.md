# html_entity_decode




<div class="phpcode"><span class="html">
If you need something that converts &amp;#[0-9]+ entities to UTF-8, this is simple and works:
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/* Entity crap. /
<br>$input = &quot;Fovi&amp;#269;&quot;;
<br>
<br>$output = preg_replace_callback(&quot;/(&amp;#[0-9]+;)/&quot;, function($m) { return mb_convert_encoding($m[1], &quot;UTF-8&quot;, &quot;HTML-ENTITIES&quot;); }, $input);
<br>
<br>/* Plain UTF-8. */
<br></span><span class="keyword">echo </span><span class="default">$output</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Use the following to decode all entities:<br><span class="default">&lt;?php html_entity_decode</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">ENT_QUOTES </span><span class="keyword">| </span><span class="default">ENT_XML1</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">) </span><span class="default">?&gt;<br></span><br>I&apos;ve checked these special entities: <br>- double quotes (&amp;#34;)<br>- single quotes (&amp;#39; and &amp;apos;) <br>- non printable chars (e.g. &amp;#13;)<br>With other $flags some or all won&apos;t be decoded.<br><br>It seems that ENT_XML1 and ENT_XHTML are identical when decoding.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.html-entity-decode.php)

**[⬆ to root](/)**