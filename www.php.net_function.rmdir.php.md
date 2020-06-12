# rmdir




<div class="phpcode"><span class="html">
Glob function doesn&apos;t return the hidden files, therefore scandir can be more useful, when trying to delete recursively a tree.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">public static function </span><span class="default">delTree</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">) {
<br>&#xA0;&#xA0; </span><span class="default">$files </span><span class="keyword">= </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">), array(</span><span class="string">&apos;.&apos;</span><span class="keyword">,</span><span class="string">&apos;..&apos;</span><span class="keyword">));
<br>&#xA0; &#xA0; foreach (</span><span class="default">$files </span><span class="keyword">as </span><span class="default">$file</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">)) ? </span><span class="default">delTree</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">) : </span><span class="default">unlink</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);
<br>&#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Never ever use jurchiks101 at gmail dot com code!!! It contains command injection vulnerability!!!<br>If you want to do it that way, use something like this instead:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">PHP_OS </span><span class="keyword">=== </span><span class="string">&apos;Windows&apos;</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&quot;rd /s /q %s&quot;</span><span class="keyword">, </span><span class="default">escapeshellarg</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">)));<br>}<br>else<br>{<br>&#xA0; &#xA0; </span><span class="default">exec</span><span class="keyword">(</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&quot;rm -rf %s&quot;</span><span class="keyword">, </span><span class="default">escapeshellarg</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">)));<br>}<br></span><span class="default">?&gt;<br></span><br>Note the escapeshellarg usage to escape any possible unwanted character, this avoids putting commands in $path variable so the possibility of someone &quot;pwning&quot; the server with this code</span>
</div>
  

#


<div class="phpcode"><span class="html">
The function delTree is dangerous when you dont take really care. I for example always deleted a temporary directory with it. Everthing went fine until the moment where the var containing this temporary directory wasnt set. The var didnt contain the path but an empty string. The function delTree&#xA0; was called and deleted all the files at my host!<br>So dont use this function when you dont have a proper handling coded. Dont think about using this function only for testing without such a handling.<br>Luckily nothing is lost because I had the local copy...</span>
</div>
  

#


<div class="phpcode"><span class="html">
some implementations of recursive folder delete don&apos;t work so well (some give warnings, other don&apos;t delete hidden files etc).<br><br>this one is working fine:<br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$dir </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">);<br>&#xA0; &#xA0; while(</span><span class="default">false </span><span class="keyword">!== ( </span><span class="default">$file </span><span class="keyword">= </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (( </span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&apos;.&apos; </span><span class="keyword">) &amp;&amp; ( </span><span class="default">$file </span><span class="keyword">!= </span><span class="string">&apos;..&apos; </span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$full </span><span class="keyword">= </span><span class="default">$src </span><span class="keyword">. </span><span class="string">&apos;/&apos; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$full</span><span class="keyword">) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="default">$full</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$full</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I was working on some Dataoperation, and just wanted to share an OOP method with you.<br><br>It just removes any contents of a Directory but not the target Directory itself! Its really nice if you want to clean a BackupDirectory or Log.<br><br>Also you can test on it if something went wrong or if it just done its Work!<br><br>I have it in a FileHandler class for example, enjoy!<br><br><span class="default">&lt;?php <br><br>&#xA0; </span><span class="keyword">public function </span><span class="default">deleteContent</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; try{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$iterator </span><span class="keyword">= new </span><span class="default">DirectoryIterator</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach ( </span><span class="default">$iterator </span><span class="keyword">as </span><span class="default">$fileinfo </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$fileinfo</span><span class="keyword">-&gt;</span><span class="default">isDot</span><span class="keyword">())continue;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$fileinfo</span><span class="keyword">-&gt;</span><span class="default">isDir</span><span class="keyword">()){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">deleteContent</span><span class="keyword">(</span><span class="default">$fileinfo</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">()))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @</span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$fileinfo</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">());<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$fileinfo</span><span class="keyword">-&gt;</span><span class="default">isFile</span><span class="keyword">()){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @</span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$fileinfo</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">());<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; } catch ( </span><span class="default">Exception $e </span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// write log<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Another simple way to recursively delete a directory that is not empty:
<br>
<br><span class="default">&lt;?php
<br> </span><span class="keyword">function </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">) {
<br>&#xA0;&#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) {
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$objects </span><span class="keyword">= </span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);
<br>&#xA0; &#xA0;&#xA0; foreach (</span><span class="default">$objects </span><span class="keyword">as </span><span class="default">$object</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0;&#xA0; if (</span><span class="default">$object </span><span class="keyword">!= </span><span class="string">&quot;.&quot; </span><span class="keyword">&amp;&amp; </span><span class="default">$object </span><span class="keyword">!= </span><span class="string">&quot;..&quot;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (</span><span class="default">filetype</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="string">&quot;/&quot;</span><span class="keyword">.</span><span class="default">$object</span><span class="keyword">) == </span><span class="string">&quot;dir&quot;</span><span class="keyword">) </span><span class="default">rrmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="string">&quot;/&quot;</span><span class="keyword">.</span><span class="default">$object</span><span class="keyword">); else </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="string">&quot;/&quot;</span><span class="keyword">.</span><span class="default">$object</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">reset</span><span class="keyword">(</span><span class="default">$objects</span><span class="keyword">);
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);
<br>&#xA0;&#xA0; }
<br> }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is rather dangerous to recurse into symbolically linked directories. The delTree should be modified to check for links.<br><br><span class="default">&lt;?php <br></span><span class="keyword">public static function </span><span class="default">delTree</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">) { <br>&#xA0;&#xA0; </span><span class="default">$files </span><span class="keyword">= </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">), array(</span><span class="string">&apos;.&apos;</span><span class="keyword">,</span><span class="string">&apos;..&apos;</span><span class="keyword">)); <br>&#xA0; &#xA0; foreach (</span><span class="default">$files </span><span class="keyword">as </span><span class="default">$file</span><span class="keyword">) { <br>&#xA0; &#xA0; &#xA0; (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">) &amp;&amp; !</span><span class="default">is_link</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) ? </span><span class="default">delTree</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">) : </span><span class="default">unlink</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$file</span><span class="string">&quot;</span><span class="keyword">); <br>&#xA0; &#xA0; } <br>&#xA0; &#xA0; return </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">); <br>&#xA0; } <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.rmdir.php)

**[⬆ to root](/)**