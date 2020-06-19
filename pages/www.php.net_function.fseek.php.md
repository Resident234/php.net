# fseek




<div class="phpcode"><span class="html">
JUST TO QUOTE AND POINT THIS OUT:<br><br>!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!<br>3. if you&apos;re using fseek() to write data to a file, remember to open the file in &quot;r+&quot; <br>mode, example:<br><br>&#xA0; $fp=fopen($filename,&quot;r+&quot;);<br><br>DON&apos;T open the file in mode &quot;a&quot; (for append), because it puts<br> the file pointer at the end of the file and doesn&apos;t let you <br>fseek earlier positions in the file (it didn&apos;t for me!). Also, <br>don&apos;t open the file in mode &quot;w&quot; -- although this puts you at <br>the beginning of the file -- because it wipes out all data in <br>the file. <br><br>!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!<br><br>Took me half a day to figure :/</span>
</div>
  

#


<div class="phpcode"><span class="html">
The official docs indicate that not all streams are seekable.<br>You can try to seek anyway and handle failure:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">, </span><span class="default">$offset</span><span class="keyword">, </span><span class="default">SEEK_CUR</span><span class="keyword">) === -</span><span class="default">1</span><span class="keyword">) {<br>&#xA0; </span><span class="comment">// whatever<br></span><span class="keyword">} <br></span><span class="default">?&gt;<br></span><br>Or, you can use the stream_get_meta_data function:<br><a href="http://php.net/stream_get_meta_data" rel="nofollow" target="_blank">http://php.net/stream_get_meta_data</a><br><br><span class="default">&lt;?php <br></span><span class="keyword">function </span><span class="default">fseekable</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">) {<br>&#xA0; </span><span class="default">$meta </span><span class="keyword">= </span><span class="default">stream_get_meta_data</span><span class="keyword">(</span><span class="default">$stream</span><span class="keyword">);<br>&#xA0; return </span><span class="default">$meta</span><span class="keyword">[</span><span class="string">&apos;seekable&apos;</span><span class="keyword">];<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I want to give my contribution about the &quot;read last lines from a file&quot; topic. I&apos;ve done some researches (starting from here, really) and run many tests for different algorithms and scenarios, and came up with this:<br><br>What is the best way in PHP to read last lines from a file?<br><a href="http://stackoverflow.com/a/15025877/995958" rel="nofollow" target="_blank">http://stackoverflow.com/a/15025877/995958</a><br><br>In that mini-article I tried to analyze all different methods and their performance over different files.<br><br>Hope it helps.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Write Dummy File 4GB in Php 32bits (X86)<br>if you want write more GB File (&gt;4GB), use Php(X64) .<br>this file is created in 0.0041329860687256 second<br><br>CreatFileDummy(&apos;data_test.txt&apos;,4294967296);<br><br>FUNCTION CreatFileDummy($file_name,$size) {&#xA0; &#xA0; <br>// 32bits 4&#xA0;294&#xA0;967&#xA0;296 bytes MAX Size <br>&#xA0; &#xA0; $f = fopen($file_name, &apos;wb&apos;);<br>&#xA0; &#xA0; if($size &gt;= 1000000000)&#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $z = ($size / 1000000000);&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if (is_float($z))&#xA0; { <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $z = round($z,0);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($f, ( $size - ($z * 1000000000) -1 ), SEEK_END);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($f, &quot;\0&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; while(--$z &gt; -1) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($f, 999999999, SEEK_END);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($f, &quot;\0&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; } <br>&#xA0; &#xA0; else {<br>&#xA0; &#xA0; &#xA0; &#xA0; fseek($f, $size - 1, SEEK_END);<br>&#xA0; &#xA0; &#xA0; &#xA0; fwrite($f, &quot;\0&quot;);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; fclose($f);<br><br>Return true;<br>}<br><br>Synx</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fseek.php)

**[To root](/README.md)**