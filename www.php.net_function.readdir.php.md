# readdir




<div class="phpcode"><span class="html">
A function I created to non-recursively get the path of all files and folders including sub-directories of a given folder.
<br>Though I have not tested it completely, it seems to be working.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">/**
<br> * Finds path, relative to the given root folder, of all files and directories in the given directory and its sub-directories non recursively.
<br> * Will return an array of the form 
<br> * array(
<br> *&#xA0;&#xA0; &apos;files&apos; =&gt; [],
<br> *&#xA0;&#xA0; &apos;dirs&apos;&#xA0; =&gt; [],
<br> * )
<br> * @author sreekumar
<br> * @param string $root
<br> * @result array
<br> */
<br></span><span class="keyword">function </span><span class="default">read_all_files</span><span class="keyword">(</span><span class="default">$root </span><span class="keyword">= </span><span class="string">&apos;.&apos;</span><span class="keyword">){
<br>&#xA0; </span><span class="default">$files&#xA0; </span><span class="keyword">= array(</span><span class="string">&apos;files&apos;</span><span class="keyword">=&gt;array(), </span><span class="string">&apos;dirs&apos;</span><span class="keyword">=&gt;array());
<br>&#xA0; </span><span class="default">$directories&#xA0; </span><span class="keyword">= array();
<br>&#xA0; </span><span class="default">$last_letter&#xA0; </span><span class="keyword">= </span><span class="default">$root</span><span class="keyword">[</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$root</span><span class="keyword">)-</span><span class="default">1</span><span class="keyword">];
<br>&#xA0; </span><span class="default">$root&#xA0; </span><span class="keyword">= (</span><span class="default">$last_letter </span><span class="keyword">== </span><span class="string">&apos;\\&apos; </span><span class="keyword">|| </span><span class="default">$last_letter </span><span class="keyword">== </span><span class="string">&apos;/&apos;</span><span class="keyword">) ? </span><span class="default">$root </span><span class="keyword">: </span><span class="default">$root</span><span class="keyword">.</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">;
<br>&#xA0; 
<br>&#xA0; </span><span class="default">$directories</span><span class="keyword">[]&#xA0; = </span><span class="default">$root</span><span class="keyword">;
<br>&#xA0; 
<br>&#xA0; while (</span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$directories</span><span class="keyword">)) {
<br>&#xA0; &#xA0; </span><span class="default">$dir&#xA0; </span><span class="keyword">= </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$directories</span><span class="keyword">);
<br>&#xA0; &#xA0; if (</span><span class="default">$handle </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; while (</span><span class="default">false </span><span class="keyword">!== (</span><span class="default">$file </span><span class="keyword">= </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$file </span><span class="keyword">== </span><span class="string">&apos;.&apos; </span><span class="keyword">|| </span><span class="default">$file </span><span class="keyword">== </span><span class="string">&apos;..&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$file&#xA0; </span><span class="keyword">= </span><span class="default">$dir</span><span class="keyword">.</span><span class="default">$file</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$directory_path </span><span class="keyword">= </span><span class="default">$file</span><span class="keyword">.</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$directories</span><span class="keyword">, </span><span class="default">$directory_path</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$files</span><span class="keyword">[</span><span class="string">&apos;dirs&apos;</span><span class="keyword">][]&#xA0; = </span><span class="default">$directory_path</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; } elseif (</span><span class="default">is_file</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$files</span><span class="keyword">[</span><span class="string">&apos;files&apos;</span><span class="keyword">][]&#xA0; = </span><span class="default">$file</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; return </span><span class="default">$files</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.readdir.php)

**[To root](/README.md)**