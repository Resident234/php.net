# file




<div class="phpcode"><span class="html">
To write all the lines of the file in other words to read the file line by line you can write the code like this:<br><span class="default">&lt;?php<br>$names</span><span class="keyword">=</span><span class="default">file</span><span class="keyword">(</span><span class="string">&apos;name.txt&apos;</span><span class="keyword">);<br></span><span class="comment">// To check the number of lines <br></span><span class="keyword">echo </span><span class="default">count</span><span class="keyword">(</span><span class="default">$names</span><span class="keyword">).</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>foreach(</span><span class="default">$names </span><span class="keyword">as </span><span class="default">$name</span><span class="keyword">)<br>{<br>&#xA0;&#xA0; echo </span><span class="default">$name</span><span class="keyword">.</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>this example is so basic to understand how it&apos;s working. I hope it will help many beginners.<br><br>Regards,<br>Bingo</span>
</div>
  

#


<div class="phpcode"><span class="html">
this may be obvious, but it took me a while to figure out what I was doing wrong. So I wanted to share. I have a file on my &quot;c:\&quot; drive. How do I file() it? <br><br>Don&apos;t forget the backslash is special and you have to &quot;escape&quot; the backslash i.e. &quot;\\&quot;:<br><br><span class="default">&lt;?php<br><br>$lines </span><span class="keyword">= </span><span class="default">file</span><span class="keyword">(</span><span class="string">&quot;C:\\Documents and Settings\\myfile.txt&quot;</span><span class="keyword">);<br><br>foreach(</span><span class="default">$lines </span><span class="keyword">as </span><span class="default">$line</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; echo(</span><span class="default">$line</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span> <br><br>hope this helps...</span>
</div>
  

#


<div class="phpcode"><span class="html">
If the file you are reading is in CSV format do not use file(), use fgetcsv().&#xA0; file() will split the file by each newline that it finds, even newlines that appear within a field (i.e. within quotations).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.file.php)

**[⬆ to root](/)**