# filesize




<div class="phpcode"><span class="html">
Extremely simple function to get human filesize.<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">human_filesize</span><span class="keyword">(</span><span class="default">$bytes</span><span class="keyword">, </span><span class="default">$decimals </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">) {<br>&#xA0; </span><span class="default">$sz </span><span class="keyword">= </span><span class="string">&apos;BKMGTP&apos;</span><span class="keyword">;<br>&#xA0; </span><span class="default">$factor </span><span class="keyword">= </span><span class="default">floor</span><span class="keyword">((</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$bytes</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">) / </span><span class="default">3</span><span class="keyword">);<br>&#xA0; return </span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&quot;%.</span><span class="keyword">{</span><span class="default">$decimals</span><span class="keyword">}</span><span class="string">f&quot;</span><span class="keyword">, </span><span class="default">$bytes </span><span class="keyword">/ </span><span class="default">pow</span><span class="keyword">(</span><span class="default">1024</span><span class="keyword">, </span><span class="default">$factor</span><span class="keyword">)) . @</span><span class="default">$sz</span><span class="keyword">[</span><span class="default">$factor</span><span class="keyword">];<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you recently appended something to file, and closed it then this method will not show appended data:<br><span class="default">&lt;?php<br></span><span class="comment">// get contents of a file into a string<br></span><span class="default">$filename </span><span class="keyword">= </span><span class="string">&quot;/usr/local/something.txt&quot;</span><span class="keyword">;<br></span><span class="default">$handle </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">);<br></span><span class="default">$contents </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">));<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>You should insert a call to clearstatcache() before calling filesize()<br>I&apos;ve spent two hours to find that =/</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">/** <br>* Converts bytes into human readable file size. <br>* <br>* @param string $bytes <br>* @return string human readable file size (2,87 &#x41C;&#x431;)<br>* @author Mogilev Arseny <br>*/ <br></span><span class="keyword">function </span><span class="default">FileSizeConvert</span><span class="keyword">(</span><span class="default">$bytes</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$bytes </span><span class="keyword">= </span><span class="default">floatval</span><span class="keyword">(</span><span class="default">$bytes</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$arBytes </span><span class="keyword">= array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">0 </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;UNIT&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;TB&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;VALUE&quot; </span><span class="keyword">=&gt; </span><span class="default">pow</span><span class="keyword">(</span><span class="default">1024</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">1 </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;UNIT&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;GB&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;VALUE&quot; </span><span class="keyword">=&gt; </span><span class="default">pow</span><span class="keyword">(</span><span class="default">1024</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">2 </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;UNIT&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;MB&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;VALUE&quot; </span><span class="keyword">=&gt; </span><span class="default">pow</span><span class="keyword">(</span><span class="default">1024</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">3 </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;UNIT&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;KB&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;VALUE&quot; </span><span class="keyword">=&gt; </span><span class="default">1024<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">4 </span><span class="keyword">=&gt; array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;UNIT&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;B&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;VALUE&quot; </span><span class="keyword">=&gt; </span><span class="default">1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; );<br><br>&#xA0; &#xA0; foreach(</span><span class="default">$arBytes </span><span class="keyword">as </span><span class="default">$arItem</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$bytes </span><span class="keyword">&gt;= </span><span class="default">$arItem</span><span class="keyword">[</span><span class="string">&quot;VALUE&quot;</span><span class="keyword">])<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">$bytes </span><span class="keyword">/ </span><span class="default">$arItem</span><span class="keyword">[</span><span class="string">&quot;VALUE&quot;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;.&quot;</span><span class="keyword">, </span><span class="string">&quot;,&quot; </span><span class="keyword">, </span><span class="default">strval</span><span class="keyword">(</span><span class="default">round</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">))).</span><span class="string">&quot; &quot;</span><span class="keyword">.</span><span class="default">$arItem</span><span class="keyword">[</span><span class="string">&quot;UNIT&quot;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">/**<br> * Return file size (even for file &gt; 2 Gb)<br> * For file size over PHP_INT_MAX (2 147 483 647), PHP filesize function loops from -PHP_INT_MAX to PHP_INT_MAX.<br> *<br> * @param string $path Path of the file<br> * @return mixed File size or false if error<br> */<br></span><span class="keyword">function </span><span class="default">realFileSize</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if (!</span><span class="default">file_exists</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">= </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (!(</span><span class="default">$file </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">)))<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if (</span><span class="default">$size </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">)<br>&#xA0; &#xA0; {</span><span class="comment">//Check if it really is a small file (&lt; 2 GB)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">SEEK_END</span><span class="keyword">) === </span><span class="default">0</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {</span><span class="comment">//It really is a small file<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$size</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">//Quickly jump the first 2 GB with fseek. After that fseek is not working on 32 bit php (it uses int internally)<br>&#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">= </span><span class="default">PHP_INT_MAX </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; if (</span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="default">PHP_INT_MAX </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">) !== </span><span class="default">0</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$length </span><span class="keyword">= </span><span class="default">1024 </span><span class="keyword">* </span><span class="default">1024</span><span class="keyword">;<br>&#xA0; &#xA0; while (!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">))<br>&#xA0; &#xA0; {</span><span class="comment">//Read the file until end<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$read </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="default">$length</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">= </span><span class="default">bcadd</span><span class="keyword">(</span><span class="default">$size</span><span class="keyword">, </span><span class="default">$length</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">= </span><span class="default">bcsub</span><span class="keyword">(</span><span class="default">$size</span><span class="keyword">, </span><span class="default">$length</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">= </span><span class="default">bcadd</span><span class="keyword">(</span><span class="default">$size</span><span class="keyword">, </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$read</span><span class="keyword">));<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$size</span><span class="keyword">;<br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is my super fast method of getting &gt;2GB files to output the correct byte size on any version of windows works with both 32Bit and 64Bit.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">find_filesize</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">PHP_OS</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">) == </span><span class="string">&quot;WIN&quot;</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="string">&apos;for %I in (&quot;&apos;</span><span class="keyword">.</span><span class="default">$file</span><span class="keyword">.</span><span class="string">&apos;&quot;) do @echo %~zI&apos;</span><span class="keyword">, </span><span class="default">$output</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$return </span><span class="keyword">= </span><span class="default">$output</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$return </span><span class="keyword">= </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$return</span><span class="keyword">;<br>}<br><br></span><span class="comment">//Usage : find_filesize(&quot;path&quot;);<br>//Example :<br></span><span class="keyword">echo </span><span class="string">&quot;File size is : &quot;</span><span class="keyword">.</span><span class="default">find_filesize</span><span class="keyword">(</span><span class="string">&quot;D:\Server\movie.mp4&quot;</span><span class="keyword">).</span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The simplest and most efficient implemention for getting remote filesize:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">remote_filesize</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">) {<br>&#xA0; &#xA0; static </span><span class="default">$regex </span><span class="keyword">= </span><span class="string">&apos;/^Content-Length: *+\K\d++$/im&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; if (!</span><span class="default">$fp </span><span class="keyword">= @</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; if (<br>&#xA0; &#xA0; &#xA0; &#xA0; isset(</span><span class="default">$http_response_header</span><span class="keyword">) &amp;&amp;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="default">$regex</span><span class="keyword">, </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$http_response_header</span><span class="keyword">), </span><span class="default">$matches</span><span class="keyword">)<br>&#xA0; &#xA0; ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return (int)</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">stream_get_contents</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.filesize.php)

**[To root](/README.md)**