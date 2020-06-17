# scandir




<div class="phpcode"><span class="html">
Easy way to get rid of the dots that scandir() picks up in Linux environments:<br><br><span class="default">&lt;?php<br>$directory </span><span class="keyword">= </span><span class="string">&apos;/path/to/my/directory&apos;</span><span class="keyword">;<br></span><span class="default">$scanned_directory </span><span class="keyword">= </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$directory</span><span class="keyword">), array(</span><span class="string">&apos;..&apos;</span><span class="keyword">, </span><span class="string">&apos;.&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is my 2 cents. I wanted to create an array of my directory structure recursively. I wanted to easely access data in a certain directory using foreach. I came up with the following:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">dirToArray</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">) {
<br>&#xA0;&#xA0; 
<br>&#xA0;&#xA0; </span><span class="default">$result </span><span class="keyword">= array();
<br>
<br>&#xA0;&#xA0; </span><span class="default">$cdir </span><span class="keyword">= </span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);
<br>&#xA0;&#xA0; foreach (</span><span class="default">$cdir </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; if (!</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">,array(</span><span class="string">&quot;.&quot;</span><span class="keyword">,</span><span class="string">&quot;..&quot;</span><span class="keyword">)))
<br>&#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$dir </span><span class="keyword">. </span><span class="default">DIRECTORY_SEPARATOR </span><span class="keyword">. </span><span class="default">$value</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$value</span><span class="keyword">] = </span><span class="default">dirToArray</span><span class="keyword">(</span><span class="default">$dir </span><span class="keyword">. </span><span class="default">DIRECTORY_SEPARATOR </span><span class="keyword">. </span><span class="default">$value</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } 
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0;&#xA0; }
<br>&#xA0;&#xA0; 
<br>&#xA0;&#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Output
<br>Array
<br>(
<br>&#xA0;&#xA0; [subdir1] =&gt; Array
<br>&#xA0;&#xA0; (
<br>&#xA0; &#xA0; &#xA0; [0] =&gt; file1.txt
<br>&#xA0; &#xA0; &#xA0; [subsubdir] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; [0] =&gt; file2.txt
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; [1] =&gt; file3.txt
<br>&#xA0; &#xA0; &#xA0; )
<br>&#xA0;&#xA0; )
<br>&#xA0;&#xA0; [subdir2] =&gt; Array
<br>&#xA0;&#xA0; (
<br>&#xA0; &#xA0; [0] =&gt; file4.txt
<br>&#xA0;&#xA0; }
<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Someone wrote that array_slice could be used to quickly remove directory entries &quot;.&quot; and &quot;..&quot;. However, &quot;-&quot; is a valid entry that would come before those, so array_slice would remove the wrong entries.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Fastest way to get a list of files without dots.<br><span class="default">&lt;?php<br>$files </span><span class="keyword">= </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">scandir</span><span class="keyword">(</span><span class="string">&apos;/path/to/directory/&apos;</span><span class="keyword">), </span><span class="default">2</span><span class="keyword">);</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Needed something that could return the contents of single or multiple directories, recursively or non-recursively,<br>for all files or specified file extensions that would be<br>accessible easily from any scope or script.<br><br>And I wanted to allow overloading cause sometimes I&apos;m too lazy to pass all params.<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">scanDir </span><span class="keyword">{<br>&#xA0; &#xA0; static private </span><span class="default">$directories</span><span class="keyword">, </span><span class="default">$files</span><span class="keyword">, </span><span class="default">$ext_filter</span><span class="keyword">, </span><span class="default">$recursive</span><span class="keyword">;<br><br></span><span class="comment">// ----------------------------------------------------------------------------------------------<br>&#xA0; &#xA0; // scan(dirpath::string|array, extensions::string|array, recursive::true|false)<br>&#xA0; &#xA0; </span><span class="keyword">static public function </span><span class="default">scan</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Initialize defaults<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$recursive </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$directories </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$files </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$ext_filter </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Check we have minimum parameters<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(!</span><span class="default">$args </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">()){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die(</span><span class="string">&quot;Must provide a path string or array of path strings&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) != </span><span class="string">&quot;string&quot; </span><span class="keyword">&amp;&amp; </span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) != </span><span class="string">&quot;array&quot;</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; die(</span><span class="string">&quot;Must provide a path string or array of path strings&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Check if recursive scan | default action: no sub-directories<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(isset(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]) &amp;&amp; </span><span class="default">$args</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] == </span><span class="default">true</span><span class="keyword">){</span><span class="default">self</span><span class="keyword">::</span><span class="default">$recursive </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;}<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Was a filter on file extensions included? | default action: return all file types<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(isset(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">])){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) == </span><span class="string">&quot;array&quot;</span><span class="keyword">){</span><span class="default">self</span><span class="keyword">::</span><span class="default">$ext_filter </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;strtolower&apos;</span><span class="keyword">, </span><span class="default">$args</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]);}<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) == </span><span class="string">&quot;string&quot;</span><span class="keyword">){</span><span class="default">self</span><span class="keyword">::</span><span class="default">$ext_filter</span><span class="keyword">[] = </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]);}<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Grab path(s)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">verifyPaths</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">self</span><span class="keyword">::</span><span class="default">$files</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; static private function </span><span class="default">verifyPaths</span><span class="keyword">(</span><span class="default">$paths</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$path_errors </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$paths</span><span class="keyword">) == </span><span class="string">&quot;string&quot;</span><span class="keyword">){</span><span class="default">$paths </span><span class="keyword">= array(</span><span class="default">$paths</span><span class="keyword">);}<br><br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$paths </span><span class="keyword">as </span><span class="default">$path</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$directories</span><span class="keyword">[] = </span><span class="default">$path</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dirContents </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">find_contents</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$path_errors</span><span class="keyword">[] = </span><span class="default">$path</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$path_errors</span><span class="keyword">){echo </span><span class="string">&quot;The following directories do not exists&lt;br /&gt;&quot;</span><span class="keyword">;die(</span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$path_errors</span><span class="keyword">));}<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">// This is how we scan directories<br>&#xA0; &#xA0; </span><span class="keyword">static private function </span><span class="default">find_contents</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$root </span><span class="keyword">= </span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$root </span><span class="keyword">as </span><span class="default">$value</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$value </span><span class="keyword">=== </span><span class="string">&apos;.&apos; </span><span class="keyword">|| </span><span class="default">$value </span><span class="keyword">=== </span><span class="string">&apos;..&apos;</span><span class="keyword">) {continue;}<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_file</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">.</span><span class="default">$value</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">self</span><span class="keyword">::</span><span class="default">$ext_filter </span><span class="keyword">|| </span><span class="default">in_array</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">pathinfo</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">.</span><span class="default">$value</span><span class="keyword">, </span><span class="default">PATHINFO_EXTENSION</span><span class="keyword">)), </span><span class="default">self</span><span class="keyword">::</span><span class="default">$ext_filter</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$files</span><span class="keyword">[] = </span><span class="default">$result</span><span class="keyword">[] = </span><span class="default">$dir</span><span class="keyword">.</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">.</span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">self</span><span class="keyword">::</span><span class="default">$recursive</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">self</span><span class="keyword">::</span><span class="default">find_contents</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="default">DIRECTORY_SEPARATOR</span><span class="keyword">.</span><span class="default">$value</span><span class="keyword">) as </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$files</span><span class="keyword">[] = </span><span class="default">$result</span><span class="keyword">[] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Return required for recursive search<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$result</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>Usage:<br>scanDir::scan(path(s):string|array, [file_extensions:string|array], [subfolders?:true|false]);<br><span class="default">&lt;?php<br></span><span class="comment">//Scan a single directory for all files, no sub-directories<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">scanDir</span><span class="keyword">::</span><span class="default">scan</span><span class="keyword">(</span><span class="string">&apos;D:\Websites\temp&apos;</span><span class="keyword">);<br><br></span><span class="comment">//Scan multiple directories for all files, no sub-dirs<br></span><span class="default">$dirs </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&apos;D:\folder&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="string">&apos;D:\folder2&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="string">&apos;C:\Other&apos;</span><span class="keyword">;<br>);<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">scanDir</span><span class="keyword">::</span><span class="default">scan</span><span class="keyword">(</span><span class="default">$dirs</span><span class="keyword">);<br><br></span><span class="comment">// Scan multiple directories for files with provided file extension,<br>// no sub-dirs<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">scanDir</span><span class="keyword">::</span><span class="default">scan</span><span class="keyword">(</span><span class="default">$dirs</span><span class="keyword">, </span><span class="string">&quot;jpg&quot;</span><span class="keyword">);<br></span><span class="comment">//or with an array of extensions<br></span><span class="default">$file_ext </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="string">&quot;jpg&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;bmp&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;png&quot;<br></span><span class="keyword">);<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">scanDir</span><span class="keyword">::</span><span class="default">scan</span><span class="keyword">(</span><span class="default">$dirs</span><span class="keyword">, </span><span class="default">$file_ext</span><span class="keyword">);<br><br></span><span class="comment">// Scan multiple directories for files with any extension,<br>// include files in recursive sub-folders<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">scanDir</span><span class="keyword">::</span><span class="default">scan</span><span class="keyword">(</span><span class="default">$dirs</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="comment">// Multiple dirs, with specified extensions, include sub-dir files<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">scanDir</span><span class="keyword">::</span><span class="default">scan</span><span class="keyword">(</span><span class="default">$dirs</span><span class="keyword">, </span><span class="default">$file_ext</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I needed to find a way to get the full path of all files in the directory and all subdirectories of a directory.
<br>Here&apos;s my solution: Recursive functions!
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">find_all_files</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$root </span><span class="keyword">= </span><span class="default">scandir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);
<br>&#xA0; &#xA0; foreach(</span><span class="default">$root </span><span class="keyword">as </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$value </span><span class="keyword">=== </span><span class="string">&apos;.&apos; </span><span class="keyword">|| </span><span class="default">$value </span><span class="keyword">=== </span><span class="string">&apos;..&apos;</span><span class="keyword">) {continue;}
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_file</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$value</span><span class="string">&quot;</span><span class="keyword">)) {</span><span class="default">$result</span><span class="keyword">[]=</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$value</span><span class="string">&quot;</span><span class="keyword">;continue;}
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">find_all_files</span><span class="keyword">(</span><span class="string">&quot;</span><span class="default">$dir</span><span class="string">/</span><span class="default">$value</span><span class="string">&quot;</span><span class="keyword">) as </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[]=</span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.scandir.php)

**[To root](/README.md)**