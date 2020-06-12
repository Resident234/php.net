# dBase




<div class="phpcode"><span class="html">
Unfortunately the dbase functions are not compiled into my commercial server&apos;s php and I needed to read some geo data in shape files, which include data in dbfs.
<br>
<br>So maybe this will help some others:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">echo_dbf</span><span class="keyword">(</span><span class="default">$dbfname</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$fdbf </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$dbfname</span><span class="keyword">,</span><span class="string">&apos;r&apos;</span><span class="keyword">); 
<br>&#xA0; &#xA0; </span><span class="default">$fields </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$buf </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fdbf</span><span class="keyword">,</span><span class="default">32</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$header</span><span class="keyword">=</span><span class="default">unpack</span><span class="keyword">( </span><span class="string">&quot;VRecordCount/vFirstRecord/vRecordLength&quot;</span><span class="keyword">, </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$buf</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">,</span><span class="default">8</span><span class="keyword">));
<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Header: &apos;</span><span class="keyword">.</span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">).</span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$goon </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">; 
<br>&#xA0; &#xA0; </span><span class="default">$unpackString</span><span class="keyword">=</span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; while (</span><span class="default">$goon </span><span class="keyword">&amp;&amp; !</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fdbf</span><span class="keyword">)) { </span><span class="comment">// read fields:
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$buf </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fdbf</span><span class="keyword">,</span><span class="default">32</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$buf</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">)==</span><span class="default">chr</span><span class="keyword">(</span><span class="default">13</span><span class="keyword">)) {</span><span class="default">$goon</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;} </span><span class="comment">// end of field list
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$field</span><span class="keyword">=</span><span class="default">unpack</span><span class="keyword">( </span><span class="string">&quot;a11fieldname/A1fieldtype/Voffset/Cfieldlen/Cfielddec&quot;</span><span class="keyword">, </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$buf</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">18</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Field: &apos;</span><span class="keyword">.</span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$field</span><span class="keyword">).</span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$unpackString</span><span class="keyword">.=</span><span class="string">&quot;A</span><span class="default">$field</span><span class="keyword">[</span><span class="default">fieldlen</span><span class="keyword">]</span><span class="default">$field</span><span class="keyword">[</span><span class="default">fieldname</span><span class="keyword">]</span><span class="string">/&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$fields</span><span class="keyword">, </span><span class="default">$field</span><span class="keyword">);}}
<br>&#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$fdbf</span><span class="keyword">, </span><span class="default">$header</span><span class="keyword">[</span><span class="string">&apos;FirstRecord&apos;</span><span class="keyword">]+</span><span class="default">1</span><span class="keyword">); </span><span class="comment">// move back to the start of the first record (after the field definitions)
<br>&#xA0; &#xA0; </span><span class="keyword">for (</span><span class="default">$i</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">&lt;=</span><span class="default">$header</span><span class="keyword">[</span><span class="string">&apos;RecordCount&apos;</span><span class="keyword">]; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$buf </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fdbf</span><span class="keyword">,</span><span class="default">$header</span><span class="keyword">[</span><span class="string">&apos;RecordLength&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$record</span><span class="keyword">=</span><span class="default">unpack</span><span class="keyword">(</span><span class="default">$unpackString</span><span class="keyword">,</span><span class="default">$buf</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;record: &apos;</span><span class="keyword">.</span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$record</span><span class="keyword">).</span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$i</span><span class="keyword">.</span><span class="default">$buf</span><span class="keyword">.</span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;} </span><span class="comment">//raw record
<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fdbf</span><span class="keyword">); }
<br></span><span class="default">?&gt;
<br></span>
<br>This function simply dumps an entire file using echo and json_encode, so you can tweak it to your own needs... (eg random access would just be a matter of changing the seek to : fseek($fdbf, $header[&apos;FirstRecord&apos;]+1 +($header[&apos;RecordLength&apos;]* $desiredrecord0based); removing the for loop and returning $record
<br>
<br>This function doesn&apos;t do any type conversion, but it does extract the type if you need to play with dates, or tidy up the numbers etc.
<br>
<br>So quick and dirty but maybe of use to somebody and illustrates the power of unpack.
<br>
<br>Erich</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.dbase.php)

**[To root](/README.md)**