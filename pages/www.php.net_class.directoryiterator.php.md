# The DirectoryIterator class




<div class="phpcode"><span class="html">
Shows us all files and catalogues in directory except &quot;.&quot; and &quot;..&quot;.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">foreach (new </span><span class="default">DirectoryIterator</span><span class="keyword">(</span><span class="string">&apos;../moodle&apos;</span><span class="keyword">) as </span><span class="default">$fileInfo</span><span class="keyword">) {<br>&#xA0; &#xA0; if(</span><span class="default">$fileInfo</span><span class="keyword">-&gt;</span><span class="default">isDot</span><span class="keyword">()) continue;<br>&#xA0; &#xA0; echo </span><span class="default">$fileInfo</span><span class="keyword">-&gt;</span><span class="default">getFilename</span><span class="keyword">() . </span><span class="string">&quot;&lt;br&gt;\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware of the behavior when using FilesystemIterator::UNIX_PATHS, it&apos;s not applied as you might expect.<br><br>I guess this flag is added especially for use on windows.<br>However, the path you construct the RecursiveDirectoryIterator or FilesystemIterator with will not be available as a unix path.<br>I can&apos;t say this is a bug, since most methods are just purely inherited from DirectoryIterator.<br><br>In my test, I&apos;d expected a complete unix path. Unfortunately... not quite as expected:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// say $folder = C:\projects\lang<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">FilesystemIterator</span><span class="keyword">::</span><span class="default">KEY_AS_PATHNAME </span><span class="keyword">| </span><span class="default">FilesystemIterator</span><span class="keyword">::</span><span class="default">CURRENT_AS_FILEINFO </span><span class="keyword">| </span><span class="default">FilesystemIterator</span><span class="keyword">::</span><span class="default">SKIP_DOTS </span><span class="keyword">| </span><span class="default">FilesystemIterator</span><span class="keyword">::</span><span class="default">UNIX_PATHS</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$d_iterator </span><span class="keyword">= new </span><span class="default">RecursiveDirectoryIterator</span><span class="keyword">(</span><span class="default">$folder</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$d_iterator</span><span class="keyword">-&gt;</span><span class="default">getPath</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>expected result: /projects/lang (or C:/projects/lang)<br>actual result: C:\projects\lang</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.directoryiterator.php)

**[To root](/README.md)**