# file_put_contents




<div class="phpcode"><span class="html">
File put contents fails if you try to put a file in a directory that doesn&apos;t exist. This creates the directory.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">file_force_contents</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$parts </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$file </span><span class="keyword">= </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$parts</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dir </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$parts </span><span class="keyword">as </span><span class="default">$part</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$dir </span><span class="keyword">.= </span><span class="string">&quot;/</span><span class="default">$part</span><span class="string">&quot;</span><span class="keyword">)) </span><span class="default">mkdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">file_put_contents</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">);<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It should be obvious that this should only be used if you&apos;re making one write, if you are writing multiple times to the same file you should handle it yourself with fopen and fwrite, the fclose when you are done writing.<br><br>Benchmark below:<br><br>file_put_contents() for 1,000,000 writes - average of 3 benchmarks:<br><br> real 0m3.932s<br> user 0m2.487s<br> sys 0m1.437s<br><br>fopen() fwrite() for 1,000,000 writes, fclose() -&#xA0; average of 3 benchmarks:<br><br> real 0m2.265s<br> user 0m1.819s<br> sys 0m0.445s</span>
</div>
  

#


<div class="phpcode"><span class="html">
A slightly simplified version of the method: <a href="http://php.net/manual/ru/function.file-put-contents.php#84180" rel="nofollow" target="_blank">http://php.net/manual/ru/function.file-put-contents.php#84180</a><br><br><span class="default">&lt;?php <br></span><span class="keyword">function </span><span class="default">file_force_contents</span><span class="keyword">( </span><span class="default">$fullPath</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">0 </span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$parts </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">( </span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="default">$fullPath </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_pop</span><span class="keyword">( </span><span class="default">$parts </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$dir </span><span class="keyword">= </span><span class="default">implode</span><span class="keyword">( </span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="default">$parts </span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if( !</span><span class="default">is_dir</span><span class="keyword">( </span><span class="default">$dir </span><span class="keyword">) )<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mkdir</span><span class="keyword">( </span><span class="default">$dir</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">, </span><span class="default">true </span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">file_put_contents</span><span class="keyword">( </span><span class="default">$fullPath</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">);<br>}<br><br></span><span class="default">file_force_contents</span><span class="keyword">( </span><span class="default">ROOT</span><span class="keyword">.</span><span class="string">&apos;/newpath/file.txt&apos;</span><span class="keyword">, </span><span class="string">&apos;message&apos;</span><span class="keyword">, </span><span class="default">LOCK_EX </span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that when saving using an FTP host, an additional stream context must be passed through telling PHP to overwrite the file.
<br>
<br><span class="default">&lt;?php
<br> </span><span class="comment">/* set the FTP hostname */
<br> </span><span class="default">$user </span><span class="keyword">= </span><span class="string">&quot;test&quot;</span><span class="keyword">;
<br> </span><span class="default">$pass </span><span class="keyword">= </span><span class="string">&quot;myFTP&quot;</span><span class="keyword">;
<br> </span><span class="default">$host </span><span class="keyword">= </span><span class="string">&quot;example.com&quot;</span><span class="keyword">;
<br> </span><span class="default">$file </span><span class="keyword">= </span><span class="string">&quot;test.txt&quot;</span><span class="keyword">;
<br> </span><span class="default">$hostname </span><span class="keyword">= </span><span class="default">$user </span><span class="keyword">. </span><span class="string">&quot;:&quot; </span><span class="keyword">. </span><span class="default">$pass </span><span class="keyword">. </span><span class="string">&quot;@&quot; </span><span class="keyword">. </span><span class="default">$host </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">;
<br>
<br> </span><span class="comment">/* the file content */
<br> </span><span class="default">$content </span><span class="keyword">= </span><span class="string">&quot;this is just a test.&quot;</span><span class="keyword">;
<br> 
<br> </span><span class="comment">/* create a stream context telling PHP to overwrite the file */
<br> </span><span class="default">$options </span><span class="keyword">= array(</span><span class="string">&apos;ftp&apos; </span><span class="keyword">=&gt; array(</span><span class="string">&apos;overwrite&apos; </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">));
<br> </span><span class="default">$stream </span><span class="keyword">= </span><span class="default">stream_context_create</span><span class="keyword">(</span><span class="default">$options</span><span class="keyword">);
<br> 
<br> </span><span class="comment">/* and finally, put the contents */
<br> </span><span class="default">file_put_contents</span><span class="keyword">(</span><span class="default">$hostname</span><span class="keyword">, </span><span class="default">$content</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$stream</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This functionality is now implemented in the PEAR package PHP_Compat.<br><br>More information about using this function without upgrading your version of PHP can be found on the below link:<br><br><a href="http://pear.php.net/package/PHP_Compat" rel="nofollow" target="_blank">http://pear.php.net/package/PHP_Compat</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s important to understand that LOCK_EX will not prevent reading the file unless you also explicitly acquire a read lock (shared locked) with the PHP &apos;flock&apos; function.<br><br>i.e. in concurrent scenarios file_get_contents may return empty if you don&apos;t wrap it like this:<br><br><span class="default">&lt;?php<br>$myfile</span><span class="keyword">=</span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&apos;test.txt&apos;</span><span class="keyword">,</span><span class="string">&apos;rt&apos;</span><span class="keyword">);<br></span><span class="default">flock</span><span class="keyword">(</span><span class="default">$myfile</span><span class="keyword">,</span><span class="default">LOCK_SH</span><span class="keyword">);<br></span><span class="default">$read</span><span class="keyword">=</span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;test.txt&apos;</span><span class="keyword">);<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$myfile</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>If you have code that does a file_get_contents on a file, changes the string, then re-saves using file_put_contents, you better be sure to do this correctly or your file will randomly wipe itself out.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.file-put-contents.php)

**[⬆ to root](/)**