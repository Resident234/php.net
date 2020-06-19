# fputcsv




<div class="phpcode"><span class="html">
If you need to send a CSV file directly to the browser, without writing in an external file, you can open the output and use fputcsv on it..<br><br><span class="default">&lt;?php<br>$out </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;php://output&apos;</span><span class="keyword">, </span><span class="string">&apos;w&apos;</span><span class="keyword">);<br></span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$out</span><span class="keyword">, array(</span><span class="string">&apos;this&apos;</span><span class="keyword">,</span><span class="string">&apos;is some&apos;</span><span class="keyword">, </span><span class="string">&apos;csv &quot;stuff&quot;, you know.&apos;</span><span class="keyword">));<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$out</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sometimes it&apos;s useful to get CSV line as string. I.e. to store it somewhere, not in on a filesystem.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">csvstr</span><span class="keyword">(array </span><span class="default">$fields</span><span class="keyword">) : </span><span class="default">string<br></span><span class="keyword">{<br>&#xA0; &#xA0; </span><span class="default">$f </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;php://memory&apos;</span><span class="keyword">, </span><span class="string">&apos;r+&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">, </span><span class="default">$fields</span><span class="keyword">) === </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">rewind</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$csv_line </span><span class="keyword">= </span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$csv_line</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need to save the output to a variable (e.g. for use within a framework) you can write to a temporary memory-wrapper and retrieve it&apos;s contents:<br><br><span class="default">&lt;?php<br></span><span class="comment">// output up to 5MB is kept in memory, if it becomes bigger it will automatically be written to a temporary file<br></span><span class="default">$csv </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;php://temp/maxmemory:&apos;</span><span class="keyword">. (</span><span class="default">5</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">), </span><span class="string">&apos;r+&apos;</span><span class="keyword">);<br><br></span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$csv</span><span class="keyword">, array(</span><span class="string">&apos;blah&apos;</span><span class="keyword">,</span><span class="string">&apos;blah&apos;</span><span class="keyword">));<br><br></span><span class="default">rewind</span><span class="keyword">(</span><span class="default">$csv</span><span class="keyword">);<br><br></span><span class="comment">// put it all in a variable<br></span><span class="default">$output </span><span class="keyword">= </span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$csv</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you want make UTF-8 file for excel, use this:<br><br>$fp = fopen($filename, &apos;w&apos;);<br>//add BOM to fix UTF-8 in Excel<br>fputs($fp, $bom =( chr(0xEF) . chr(0xBB) . chr(0xBF) ));</span>
</div>
  

#


<div class="phpcode"><span class="html">
TAB delimiting.<br><br>Using fputcsv to output a CSV with a tab delimiter is a little tricky since the delimiter field only takes one character.<br>The answer is to use the chr() function.&#xA0; The ascii code for tab is 9, so chr(9) returns a tab character.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$foo</span><span class="keyword">, </span><span class="string">&apos;\t&apos;</span><span class="keyword">);&#xA0; &#xA0; &#xA0; </span><span class="comment">//won&apos;t work<br>&#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$foo</span><span class="keyword">, </span><span class="string">&apos;&#xA0; &#xA0; &apos;</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">//won&apos;t work<br><br>&#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$foo</span><span class="keyword">, </span><span class="default">chr</span><span class="keyword">(</span><span class="default">9</span><span class="keyword">));&#xA0; &#xA0; </span><span class="comment">//works<br></span><span class="default">?&gt;<br></span><br>==================<br><br>it should be:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$foo</span><span class="keyword">, </span><span class="string">&quot;\t&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>you just forgot that single quotes are literal...meaning whatever you put there that&apos;s what will come out so &apos;\t&apos; would be same as &apos;t&apos; because \ in that case would be only used for escaping but if you use double quotes then that would work.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve created a function for quickly generating CSV files that work with Microsoft applications. In the field I learned a few things about generating CSVs that are not always obvious. First, since PHP is generally *nix-based, it makes sense that the line endings are always \n instead of \r\n. However, certain Microsoft programs (I&apos;m looking at you, Access 97), will fail to recognize the CSV properly unless each line ends with \r\n. So this function changes the line endings accordingly. Secondly, if the first column heading / value of the CSV file begins with uppercase ID, certain Microsoft programs (ahem, Excel 2007) will interpret the file as being in the SYLK format rather than CSV, as described here: <a href="http://support.microsoft.com/kb/323626" rel="nofollow" target="_blank">http://support.microsoft.com/kb/323626</a>
<br>
<br>This function accommodates for that as well, by forcibly enclosing that first value in quotes (when this doesn&apos;t occur automatically). It would be fairly simple to modify this function to use another delimiter if need be and I leave that as an exercise to the reader. So quite simply, this function is used for outputting CSV data to a CSV file in a way that is safe for use with Windows applications. It takes two parameters + one optional parameter: the location of where the file should be saved, an array of data rows, and an optional array of column headings. (Technically you could omit the headings array and just include it as the first row of the data, but it is often useful to keep this data stored in different arrays in practice.)
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">mssafe_csv</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">, </span><span class="default">$header </span><span class="keyword">= array())
<br>{
<br>&#xA0; &#xA0; if ( </span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$filepath</span><span class="keyword">, </span><span class="string">&apos;w&apos;</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$show_header </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( empty(</span><span class="default">$header</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$show_header </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">reset</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( !empty(</span><span class="default">$line</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">reset</span><span class="keyword">(</span><span class="default">$line</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$first </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">(</span><span class="default">$line</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$first</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">) == </span><span class="string">&apos;ID&apos; </span><span class="keyword">&amp;&amp; !</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/[&quot;\\s,]/&apos;</span><span class="keyword">, </span><span class="default">$first</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$line</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( empty(</span><span class="default">$line</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\&quot;</span><span class="keyword">{</span><span class="default">$first</span><span class="keyword">}</span><span class="string">\&quot;\r\n&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\&quot;</span><span class="keyword">{</span><span class="default">$first</span><span class="keyword">}</span><span class="string">\&quot;,&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$line</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">SEEK_CUR</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">reset</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$first </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$first</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">) == </span><span class="string">&apos;ID&apos; </span><span class="keyword">&amp;&amp; !</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/[&quot;\\s,]/&apos;</span><span class="keyword">, </span><span class="default">$first</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$header</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( empty(</span><span class="default">$header</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$show_header </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\&quot;</span><span class="keyword">{</span><span class="default">$first</span><span class="keyword">}</span><span class="string">\&quot;\r\n&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\&quot;</span><span class="keyword">{</span><span class="default">$first</span><span class="keyword">}</span><span class="string">\&quot;,&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">$show_header </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$header</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">SEEK_CUR</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach ( </span><span class="default">$data </span><span class="keyword">as </span><span class="default">$line </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$line</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">, </span><span class="default">SEEK_CUR</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Utility function to output a mysql query to csv with the option to write to file or send back to the browser as a csv attachment.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">query_to_csv</span><span class="keyword">(</span><span class="default">$db_conn</span><span class="keyword">, </span><span class="default">$query</span><span class="keyword">, </span><span class="default">$filename</span><span class="keyword">, </span><span class="default">$attachment </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">, </span><span class="default">$headers </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$attachment</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// send response headers to the browser<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">( </span><span class="string">&apos;Content-Type: text/csv&apos; </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">( </span><span class="string">&apos;Content-Disposition: attachment;filename=&apos;</span><span class="keyword">.</span><span class="default">$filename</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;php://output&apos;</span><span class="keyword">, </span><span class="string">&apos;w&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="string">&apos;w&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">mysql_query</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">, </span><span class="default">$db_conn</span><span class="keyword">) or die( </span><span class="default">mysql_error</span><span class="keyword">( </span><span class="default">$db_conn </span><span class="keyword">) );<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$headers</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// output header row (if at least one row exists)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysql_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$row</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// reset pointer back to beginning<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mysql_data_seek</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysql_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fputcsv</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$row</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">// Using the function<br>&#xA0; &#xA0; </span><span class="default">$sql </span><span class="keyword">= </span><span class="string">&quot;SELECT * FROM table&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="comment">// $db_conn should be a valid db handle<br><br>&#xA0; &#xA0; // output as an attachment<br>&#xA0; &#xA0; </span><span class="default">query_to_csv</span><span class="keyword">(</span><span class="default">$db_conn</span><span class="keyword">, </span><span class="default">$sql</span><span class="keyword">, </span><span class="string">&quot;test.csv&quot;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// output to file system<br>&#xA0; &#xA0; </span><span class="default">query_to_csv</span><span class="keyword">(</span><span class="default">$db_conn</span><span class="keyword">, </span><span class="default">$sql</span><span class="keyword">, </span><span class="string">&quot;test.csv&quot;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Alright, after playing a while, I&apos;m confident the following replacement function works in all cases, including the ones for which the native fputcsv function fails. If fputcsv fails to work for you (particularly with mysql csv imports), try this function as a drop-in replacement instead.
<br>
<br>Arguments to pass in are exactly the same as for fputcsv, though I have added an additional $mysql_null boolean which allows one to turn php null&apos;s into mysql-insertable nulls (by default, this add-on is disabled, thus working identically to fputcsv [except this one works!]).
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">fputcsv2 </span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, array </span><span class="default">$fields</span><span class="keyword">, </span><span class="default">$delimiter </span><span class="keyword">= </span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$enclosure </span><span class="keyword">= </span><span class="string">&apos;&quot;&apos;</span><span class="keyword">, </span><span class="default">$mysql_null </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$delimiter_esc </span><span class="keyword">= </span><span class="default">preg_quote</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="string">&apos;/&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$enclosure_esc </span><span class="keyword">= </span><span class="default">preg_quote</span><span class="keyword">(</span><span class="default">$enclosure</span><span class="keyword">, </span><span class="string">&apos;/&apos;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$output </span><span class="keyword">= array();
<br>&#xA0; &#xA0; foreach (</span><span class="default">$fields </span><span class="keyword">as </span><span class="default">$field</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$field </span><span class="keyword">=== </span><span class="default">null </span><span class="keyword">&amp;&amp; </span><span class="default">$mysql_null</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output</span><span class="keyword">[] = </span><span class="string">&apos;NULL&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$output</span><span class="keyword">[] = </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;/(?:</span><span class="keyword">${</span><span class="default">delimiter_esc</span><span class="keyword">}</span><span class="string">|</span><span class="keyword">${</span><span class="default">enclosure_esc</span><span class="keyword">}</span><span class="string">|\s)/&quot;</span><span class="keyword">, </span><span class="default">$field</span><span class="keyword">) ? (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$enclosure </span><span class="keyword">. </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$enclosure</span><span class="keyword">, </span><span class="default">$enclosure </span><span class="keyword">. </span><span class="default">$enclosure</span><span class="keyword">, </span><span class="default">$field</span><span class="keyword">) . </span><span class="default">$enclosure
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">) : </span><span class="default">$field</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">, </span><span class="default">join</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$output</span><span class="keyword">) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">);
<br>}
<br>
<br></span><span class="comment">// the _EXACT_ LOAD DATA INFILE command to use
<br>// (if you pass in something different for $delimiter
<br>// and/or $enclosure above, change them here too;
<br>// but _LEAVE ESCAPED BY EMPTY!_).
<br>/*
<br>LOAD DATA INFILE
<br>&#xA0; &#xA0; &apos;/path/to/file.csv&apos;
<br>
<br>INTO TABLE
<br>&#xA0; &#xA0; my_table
<br>
<br>FIELDS TERMINATED BY
<br>&#xA0; &#xA0; &apos;,&apos;
<br>
<br>OPTIONALLY ENCLOSED BY
<br>&#xA0; &#xA0; &apos;&quot;&apos;
<br>
<br>ESCAPED BY
<br>&#xA0; &#xA0; &apos;&apos;
<br>
<br>LINES TERMINATED BY
<br>&#xA0; &#xA0; &apos;\n&apos;
<br>*/
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fputcsv.php)

**[To root](/README.md)**