# DirectoryIterator::__construct




<div class="phpcode"><span class="html">
Here&apos;s all-in-one DirectoryIterator:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * Real Recursive Directory Iterator<br> */<br></span><span class="keyword">class </span><span class="default">RRDI </span><span class="keyword">extends </span><span class="default">RecursiveIteratorIterator </span><span class="keyword">{<br>&#xA0; </span><span class="comment">/**<br>&#xA0;&#xA0; * Creates Real Recursive Directory Iterator<br>&#xA0;&#xA0; * @param string $path<br>&#xA0;&#xA0; * @param int $flags<br>&#xA0;&#xA0; * @return DirectoryIterator<br>&#xA0;&#xA0; */<br>&#xA0; </span><span class="keyword">public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(new </span><span class="default">RecursiveDirectoryIterator</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">));<br>&#xA0; }<br>}<br><br></span><span class="comment">/**<br> * Real RecursiveDirectoryIterator Filtered Class<br> * Returns only those items which filenames match given regex<br> */<br></span><span class="keyword">class </span><span class="default">AdvancedDirectoryIterator </span><span class="keyword">extends </span><span class="default">FilterIterator </span><span class="keyword">{<br>&#xA0; </span><span class="comment">/**<br>&#xA0;&#xA0; * Regex storage<br>&#xA0;&#xA0; * @var string<br>&#xA0;&#xA0; */<br>&#xA0; </span><span class="keyword">private </span><span class="default">$regex</span><span class="keyword">;<br>&#xA0; </span><span class="comment">/**<br>&#xA0;&#xA0; * Creates new AdvancedDirectoryIterator<br>&#xA0;&#xA0; * @param string $path, prefix with &apos;-R &apos; for recursive, postfix with /[wildcards] for matching<br>&#xA0;&#xA0; * @param int $flags<br>&#xA0;&#xA0; * @return DirectoryIterator<br>&#xA0;&#xA0; */<br>&#xA0; </span><span class="keyword">public function&#xA0; </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="string">&apos;-R &apos;</span><span class="keyword">) === </span><span class="default">0</span><span class="keyword">) { </span><span class="default">$recursive </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">; </span><span class="default">$path </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">); }<br>&#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;~/?([^/]*\*[^/]*)$~&apos;</span><span class="keyword">, </span><span class="default">$path</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">)) { </span><span class="comment">// matched wildcards in filename<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$path </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, -</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) - </span><span class="default">1</span><span class="keyword">); </span><span class="comment">// strip wildcards part from path<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">regex </span><span class="keyword">= </span><span class="string">&apos;~^&apos; </span><span class="keyword">. </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;*&apos;</span><span class="keyword">, </span><span class="string">&apos;.*&apos;</span><span class="keyword">, </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">, </span><span class="string">&apos;\.&apos;</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">])) . </span><span class="string">&apos;$~&apos;</span><span class="keyword">; </span><span class="comment">// convert wildcards to regex <br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">if (!</span><span class="default">$path</span><span class="keyword">) </span><span class="default">$path </span><span class="keyword">= </span><span class="string">&apos;.&apos;</span><span class="keyword">; </span><span class="comment">// if no path given, we assume CWD<br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$recursive </span><span class="keyword">? new </span><span class="default">RRDI</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">) : new </span><span class="default">DirectoryIterator</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">));<br>&#xA0; }<br>&#xA0; </span><span class="comment">/**<br>&#xA0;&#xA0; * Checks for regex in current filename, or matches all if no regex specified<br>&#xA0;&#xA0; * @return bool<br>&#xA0;&#xA0; */<br>&#xA0; </span><span class="keyword">public function </span><span class="default">accept</span><span class="keyword">() { </span><span class="comment">// FilterIterator method<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">regex </span><span class="keyword">=== </span><span class="default">null </span><span class="keyword">? </span><span class="default">true </span><span class="keyword">: </span><span class="default">preg_match</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">regex</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">getInnerIterator</span><span class="keyword">()-&gt;</span><span class="default">getFilename</span><span class="keyword">());<br>&#xA0; }<br>}<br><br></span><span class="default">?&gt;<br></span><br>Some examples:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/* @var $i DirectoryIterator */<br><br></span><span class="keyword">foreach (new </span><span class="default">AdvancedDirectoryIterator</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">) as </span><span class="default">$i</span><span class="keyword">) echo </span><span class="default">$i</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">() . </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;<br></span><span class="comment">// will output all files and directories in CWD<br><br></span><span class="keyword">foreach (new </span><span class="default">AdvancedDirectoryIterator</span><span class="keyword">(</span><span class="string">&apos;-R *.php&apos;</span><span class="keyword">) as </span><span class="default">$i</span><span class="keyword">) echo </span><span class="default">$i</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">() . </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;<br></span><span class="comment">// will output all php files in CWD and all subdirectories<br><br></span><span class="keyword">foreach (new </span><span class="default">AdvancedDirectoryIterator</span><span class="keyword">(</span><span class="string">&apos;-R js/jquery-*.js&apos;</span><span class="keyword">) as </span><span class="default">$i</span><span class="keyword">) echo </span><span class="default">$i</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">() . </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;<br></span><span class="comment">// will output all jQuery versions in directory js, or throw an exception if directory js doesn&apos;t exist<br><br></span><span class="default">?&gt;<br></span><br>Pretty cool, huh? :)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/directoryiterator.construct.php)

**[To root](/README.md)**