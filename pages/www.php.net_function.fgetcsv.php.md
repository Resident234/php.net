# fgetcsv




<div class="phpcode"><span class="html">
If you need to set auto_detect_line_endings to deal with Mac line endings, it may seem obvious but remember it should be set before fopen, not after:<br><br>This will work:<br><span class="default">&lt;?php<br>ini_set</span><span class="keyword">(</span><span class="string">&apos;auto_detect_line_endings&apos;</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">);<br></span><span class="default">$handle </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;/path/to/file&apos;</span><span class="keyword">,</span><span class="string">&apos;r&apos;</span><span class="keyword">);<br>while ( (</span><span class="default">$data </span><span class="keyword">= </span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">) ) !== </span><span class="default">FALSE </span><span class="keyword">) {<br></span><span class="comment">//process<br></span><span class="keyword">}<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;auto_detect_line_endings&apos;</span><span class="keyword">,</span><span class="default">FALSE</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This won&apos;t, you will still get concatenated fields at the new line position:<br><span class="default">&lt;?php<br>$handle </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;/path/to/file&apos;</span><span class="keyword">,</span><span class="string">&apos;r&apos;</span><span class="keyword">);<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;auto_detect_line_endings&apos;</span><span class="keyword">,</span><span class="default">TRUE</span><span class="keyword">);<br>while ( (</span><span class="default">$data </span><span class="keyword">= </span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">) ) !== </span><span class="default">FALSE </span><span class="keyword">) {<br></span><span class="comment">//process<br></span><span class="keyword">}<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;auto_detect_line_endings&apos;</span><span class="keyword">,</span><span class="default">FALSE</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function has no special BOM handling. The first cell of the first row will inherit the BOM bytes, i.e. will be 3 bytes longer than expected. As the BOM is invisible you may not notice.<br><br>Excel on Windows, or text editors like Notepad, may add the BOM.</span>
</div>
  

#


<div class="phpcode"><span class="html">
fgetcsv seems to handle newlines within fields fine. So in fact it is not reading a line, but keeps reading untill it finds a \n-character that&apos;s not quoted as a field.<br><br>Example:<br><br><span class="default">&lt;?php<br></span><span class="comment">/* test.csv contains:<br>&quot;col 1&quot;,&quot;col2&quot;,&quot;col3&quot;<br>&quot;this<br>is<br>having<br>multiple<br>lines&quot;,&quot;this not&quot;,&quot;this also not&quot;<br>&quot;normal record&quot;,&quot;nothing to see here&quot;,&quot;no data&quot;<br>*/<br><br></span><span class="default">$handle </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;test.csv&quot;</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">);<br>while ((</span><span class="default">$data </span><span class="keyword">= </span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">)) !== </span><span class="default">FALSE</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Returns:<br>array(3) {<br>&#xA0; [0]=&gt;<br>&#xA0; string(5) &quot;col 1&quot;<br>&#xA0; [1]=&gt;<br>&#xA0; string(4) &quot;col2&quot;<br>&#xA0; [2]=&gt;<br>&#xA0; string(4) &quot;col3&quot;<br>}<br>array(3) {<br>&#xA0; [0]=&gt;<br>&#xA0; string(29) &quot;this<br>is<br>having<br>multiple<br>lines&quot;<br>&#xA0; [1]=&gt;<br>&#xA0; string(8) &quot;this not&quot;<br>&#xA0; [2]=&gt;<br>&#xA0; string(13) &quot;this also not&quot;<br>}<br>array(3) {<br>&#xA0; [0]=&gt;<br>&#xA0; string(13) &quot;normal record&quot;<br>&#xA0; [1]=&gt;<br>&#xA0; string(19) &quot;nothing to see here&quot;<br>&#xA0; [2]=&gt;<br>&#xA0; string(7) &quot;no data&quot;<br>}<br><br>This means that you can expect fgetcsv to handle newlines within fields fine. This was not clear from the documentation.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a OOP based importer similar to the one posted earlier. However, this is slightly more flexible in that you can import huge files without running out of memory, you just have to use a limit on the get() method
<br>
<br>Sample usage for small files:-
<br>-------------------------------------
<br><span class="default">&lt;?php
<br>$importer </span><span class="keyword">= new </span><span class="default">CsvImporter</span><span class="keyword">(</span><span class="string">&quot;small.txt&quot;</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">$data </span><span class="keyword">= </span><span class="default">$importer</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">();
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>
<br>Sample usage for large files:-
<br>-------------------------------------
<br><span class="default">&lt;?php
<br>$importer </span><span class="keyword">= new </span><span class="default">CsvImporter</span><span class="keyword">(</span><span class="string">&quot;large.txt&quot;</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">);
<br>while(</span><span class="default">$data </span><span class="keyword">= </span><span class="default">$importer</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">(</span><span class="default">2000</span><span class="keyword">))
<br>{
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>
<br>And heres the class:-
<br>-------------------------------------
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">CsvImporter
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; private </span><span class="default">$fp</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$parse_header</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$header</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$delimiter</span><span class="keyword">;
<br>&#xA0; &#xA0; private </span><span class="default">$length</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">//--------------------------------------------------------------------
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$file_name</span><span class="keyword">, </span><span class="default">$parse_header</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">, </span><span class="default">$delimiter</span><span class="keyword">=</span><span class="string">&quot;\t&quot;</span><span class="keyword">, </span><span class="default">$length</span><span class="keyword">=</span><span class="default">8000</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$file_name</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse_header </span><span class="keyword">= </span><span class="default">$parse_header</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">delimiter </span><span class="keyword">= </span><span class="default">$delimiter</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">length </span><span class="keyword">= </span><span class="default">$length</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">lines </span><span class="keyword">= </span><span class="default">$lines</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse_header</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">header </span><span class="keyword">= </span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">fp</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">length</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">delimiter</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">//--------------------------------------------------------------------
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">__destruct</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">fp</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">fp</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">//--------------------------------------------------------------------
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">get</span><span class="keyword">(</span><span class="default">$max_lines</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//if $max_lines is set to 0, then get all the data
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= array();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$max_lines </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line_count </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line_count </span><span class="keyword">= -</span><span class="default">1</span><span class="keyword">; </span><span class="comment">// so loop limit is ignored
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$line_count </span><span class="keyword">&lt; </span><span class="default">$max_lines </span><span class="keyword">&amp;&amp; (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">fgetcsv</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">fp</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">length</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">delimiter</span><span class="keyword">)) !== </span><span class="default">FALSE</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse_header</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">header </span><span class="keyword">as </span><span class="default">$i </span><span class="keyword">=&gt; </span><span class="default">$heading_i</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row_new</span><span class="keyword">[</span><span class="default">$heading_i</span><span class="keyword">] = </span><span class="default">$row</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data</span><span class="keyword">[] = </span><span class="default">$row_new</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$max_lines </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line_count</span><span class="keyword">++;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$data</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">//--------------------------------------------------------------------
<br>
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Only problem with fgetcsv(), at least in PHP 4.x -- any stray slash in the data that happens to come before a double-quote delimiter will break it -- ie, cause the field delimiter to be escaped. I can&apos;t find a direct way to deal with it, since fgetcsv() doesn&apos;t give you a chance to manipulate the line before it reads it and parses it...I&apos;ve had to change all occurrences of &apos;\&quot;&apos; to &apos;&quot; in the file first before feeding ot to fgetcsv(). Otherwise this is perfect for that Microsoft-CSV formula, deals gracefully with all the issues.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fgetcsv.php)

**[To root](/README.md)**