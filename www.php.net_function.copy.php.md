# copy




<div class="phpcode"><span class="html">
Having spent hours tacking down a copy() error: Permission denied , (and duly worrying about chmod on winXP) , its worth pointing out that the &apos;destination&apos; needs to contain the actual file name ! --- NOT just the path to the folder you wish to copy into.......<br>DOH !<br>hope this saves somebody hours of fruitless debugging</span>
</div>
  

#


<div class="phpcode"><span class="html">
It take me a long time to find out what the problem is when i&apos;ve got an error on copy(). It DOESN&apos;T create any directories. It only copies to existing path. So create directories before. Hope i&apos;ll help,</span>
</div>
  

#


<div class="phpcode"><span class="html">
Don&apos;t forget; you can use copy on remote files, rather than doing messy fopen stuff.&#xA0; e.g.<br><br><span class="default">&lt;?php<br></span><span class="keyword">if(!@</span><span class="default">copy</span><span class="keyword">(</span><span class="string">&apos;<a href="http://someserver.com/somefile.zip" rel="nofollow" target="_blank">http://someserver.com/somefile.zip</a>&apos;</span><span class="keyword">,</span><span class="string">&apos;./somefile.zip&apos;</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; </span><span class="default">$errors</span><span class="keyword">= </span><span class="default">error_get_last</span><span class="keyword">();<br>&#xA0; &#xA0; echo </span><span class="string">&quot;COPY ERROR: &quot;</span><span class="keyword">.</span><span class="default">$errors</span><span class="keyword">[</span><span class="string">&apos;type&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;br /&gt;\n&quot;</span><span class="keyword">.</span><span class="default">$errors</span><span class="keyword">[</span><span class="string">&apos;message&apos;</span><span class="keyword">];<br>} else {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;File copied from remote!&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A nice simple trick if you need to make sure the folder exists first:<br><br><span class="default">&lt;?php<br><br>$srcfile</span><span class="keyword">=</span><span class="string">&apos;C:\File\Whatever\Path\Joe.txt&apos;</span><span class="keyword">;<br></span><span class="default">$dstfile</span><span class="keyword">=</span><span class="string">&apos;G:\Shared\Reports\Joe.txt&apos;</span><span class="keyword">;<br></span><span class="default">mkdir</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">$dstfile</span><span class="keyword">), </span><span class="default">0777</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">copy</span><span class="keyword">(</span><span class="default">$srcfile</span><span class="keyword">, </span><span class="default">$dstfile</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>That simple.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a simple script that I use for removing and copying non-empty directories. Very useful when you are not sure what is the type of a file.<br><br>I am using these for managing folders and zip archives for my website plugins.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// removes files and non-empty directories<br></span><span class="keyword">function </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">) {<br>&#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="default">$files </span><span class="keyword">= </span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; foreach (</span><span class="default">$files </span><span class="keyword">as </span><span class="default">$file</span><span class="keyword">)<br>&#xA0; &#xA0; if (</span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&quot;.&quot; </span><span class="keyword">&amp;&amp; </span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&quot;..&quot;</span><span class="keyword">) </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; else if (</span><span class="default">file_exists</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>} <br><br></span><span class="comment">// copies files and non-empty directories<br></span><span class="keyword">function </span><span class="default">rcopy</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">, </span><span class="default">$dst</span><span class="keyword">) {<br>&#xA0; if (</span><span class="default">file_exists</span><span class="keyword">(</span><span class="default">$dst</span><span class="keyword">)) </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="default">$dst</span><span class="keyword">);<br>&#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="default">mkdir</span><span class="keyword">(</span><span class="default">$dst</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$files </span><span class="keyword">= </span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">);<br>&#xA0; &#xA0; foreach (</span><span class="default">$files </span><span class="keyword">as </span><span class="default">$file</span><span class="keyword">)<br>&#xA0; &#xA0; if (</span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&quot;.&quot; </span><span class="keyword">&amp;&amp; </span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&quot;..&quot;</span><span class="keyword">) </span><span class="default">rcopy</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$src</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">, </span><span class="string">&quot;</span><span class="default">$dst</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">); <br>&#xA0; }<br>&#xA0; else if (</span><span class="default">file_exists</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">)) </span><span class="default">copy</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">, </span><span class="default">$dst</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Cheers!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a simple recursive function to copy entire directories
<br>
<br>Note to do your own check to make sure the directory exists that you first call it on.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">recurse_copy</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">,</span><span class="default">$dst</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$dir </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">);
<br>&#xA0; &#xA0; @</span><span class="default">mkdir</span><span class="keyword">(</span><span class="default">$dst</span><span class="keyword">);
<br>&#xA0; &#xA0; while(</span><span class="default">false </span><span class="keyword">!== ( </span><span class="default">$file </span><span class="keyword">= </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (( </span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&apos;.&apos; </span><span class="keyword">) &amp;&amp; ( </span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&apos;..&apos; </span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$src </span><span class="keyword">. </span><span class="string">&apos;/&apos; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">recurse_copy</span><span class="keyword">(</span><span class="default">$src </span><span class="keyword">. </span><span class="string">&apos;/&apos; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">,</span><span class="default">$dst </span><span class="keyword">. </span><span class="string">&apos;/&apos; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else { 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">copy</span><span class="keyword">(</span><span class="default">$src </span><span class="keyword">. </span><span class="string">&apos;/&apos; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">,</span><span class="default">$dst </span><span class="keyword">. </span><span class="string">&apos;/&apos; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.copy.php)

**[⬆ to root](/)**