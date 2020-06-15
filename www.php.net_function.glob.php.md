# glob




<div class="phpcode"><span class="html">
Since I feel this is rather vague and non-helpful, I thought I&apos;d make a post detailing the mechanics of the glob regex.
<br>
<br>glob uses two special symbols that act like sort of a blend between a meta-character and a quantifier.&#xA0; These two characters are the * and ? 
<br>
<br>The ? matches 1 of any character except a /
<br>The * matches 0 or more of any character except a /
<br>
<br>If it helps, think of the * as the pcre equivalent of .* and ? as the pcre equivalent of the dot (.)
<br>
<br>Note: * and ? function independently from the previous character. For instance, if you do glob(&quot;a*.php&quot;) on the following list of files, all of the files starting with an &apos;a&apos; will be returned, but * itself would match:
<br>
<br>a.php // * matches nothing
<br>aa.php // * matches the second &apos;a&apos;
<br>ab.php // * matches &apos;b&apos;
<br>abc.php // * matches &apos;bc&apos;
<br>b.php // * matches nothing, because the starting &apos;a&apos; fails
<br>bc.php // * matches nothing, because the starting &apos;a&apos; fails
<br>bcd.php // * matches nothing, because the starting &apos;a&apos; fails
<br>
<br>It does not match just a.php and aa.php as a &apos;normal&apos; regex would, because it matches 0 or more of any character, not the character/class/group before it. 
<br>
<br>Executing glob(&quot;a?.php&quot;) on the same list of files will only return aa.php and ab.php because as mentioned, the ? is the equivalent of pcre&apos;s dot, and is NOT the same as pcre&apos;s ?, which would match 0 or 1 of the previous character.
<br>
<br>glob&apos;s regex also supports character classes and negative character classes, using the syntax [] and [^]. It will match any one character inside [] or match any one character that is not in [^].
<br>
<br>With the same list above, executing 
<br>
<br>glob(&quot;[ab]*.php) will return (all of them):
<br>a.php&#xA0; // [ab] matches &apos;a&apos;, * matches nothing
<br>aa.php // [ab] matches &apos;a&apos;, * matches 2nd &apos;a&apos;
<br>ab.php // [ab] matches &apos;a&apos;, * matches &apos;b&apos;
<br>abc.php // [ab] matches &apos;a&apos;, * matches &apos;bc&apos;
<br>b.php // [ab] matches &apos;b&apos;, * matches nothing
<br>bc.php // [ab] matches &apos;b&apos;, * matches &apos;c&apos;
<br>bcd.php // [ab] matches &apos;b&apos;, * matches &apos;cd&apos;
<br>
<br>glob(&quot;[ab].php&quot;) will return a.php and b.php
<br>
<br>glob(&quot;[^a]*.php&quot;) will return:
<br>b.php // [^a] matches &apos;b&apos;, * matches nothing
<br>bc.php // [^a] matches &apos;b&apos;, * matches &apos;c&apos;
<br>bcd.php // [^a] matches &apos;b&apos;, * matches &apos;cd&apos;
<br>
<br>glob(&quot;[^ab]*.php&quot;) will return nothing because the character class will fail to match on the first character. 
<br>
<br>You can also use ranges of characters inside the character class by having a starting and ending character with a hyphen in between.&#xA0; For example, [a-z] will match any letter between a and z, [0-9] will match any (one) number, etc.. 
<br>
<br>glob also supports limited alternation with {n1, n2, etc..}.&#xA0; You have to specify GLOB_BRACE as the 2nd argument for glob in order for it to work.&#xA0; So for example, if you executed glob(&quot;{a,b,c}.php&quot;, GLOB_BRACE) on the following list of files:
<br>
<br>a.php
<br>b.php
<br>c.php
<br>
<br>all 3 of them would return.&#xA0; Note: using alternation with single characters like that is the same thing as just doing glob(&quot;[abc].php&quot;).&#xA0; A more interesting example would be glob(&quot;te{xt,nse}.php&quot;, GLOB_BRACE) on:
<br>
<br>tent.php
<br>text.php
<br>test.php
<br>tense.php
<br>
<br>text.php and tense.php would be returned from that glob.
<br>
<br>glob&apos;s regex does not offer any kind of quantification of a specified character or character class or alternation.&#xA0; For instance, if you have the following files:
<br>
<br>a.php
<br>aa.php
<br>aaa.php
<br>ab.php
<br>abc.php
<br>b.php
<br>bc.php
<br>
<br>with pcre regex you can do ~^a+\.php$~ to return 
<br>
<br>a.php
<br>aa.php
<br>aaa.php
<br>
<br>This is not possible with glob.&#xA0; If you are trying to do something like this, you can first narrow it down with glob, and then get exact matches with a full flavored regex engine.&#xA0; For example, if you wanted all of the php files in the previous list that only have one or more &apos;a&apos; in it, you can do this:
<br>
<br><span class="default">&lt;?php
<br>&#xA0;&#xA0; $list </span><span class="keyword">= </span><span class="default">glob</span><span class="keyword">(</span><span class="string">&quot;a*.php&quot;</span><span class="keyword">);
<br>&#xA0;&#xA0; foreach (</span><span class="default">$list </span><span class="keyword">as </span><span class="default">$l</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;~^a+\.php$~&quot;</span><span class="keyword">,</span><span class="default">$file</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$files</span><span class="keyword">[] = </span><span class="default">$l</span><span class="keyword">;
<br>&#xA0;&#xA0; }
<br></span><span class="default">?&gt;
<br></span>
<br>glob also does not support lookbehinds, lookaheads, atomic groupings, capturing, or any of the &apos;higher level&apos; regex functions.
<br>
<br>glob does not support &apos;shortkey&apos; meta-characters like \w or \d.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Those of you with PHP 5 don&apos;t have to come up with these wild functions to scan a directory recursively: the SPL can do it.<br><br><span class="default">&lt;?php<br><br>$dir_iterator </span><span class="keyword">= new </span><span class="default">RecursiveDirectoryIterator</span><span class="keyword">(</span><span class="string">&quot;/path&quot;</span><span class="keyword">);<br></span><span class="default">$iterator </span><span class="keyword">= new </span><span class="default">RecursiveIteratorIterator</span><span class="keyword">(</span><span class="default">$dir_iterator</span><span class="keyword">, </span><span class="default">RecursiveIteratorIterator</span><span class="keyword">::</span><span class="default">SELF_FIRST</span><span class="keyword">);<br></span><span class="comment">// could use CHILD_FIRST if you so wish<br><br></span><span class="keyword">foreach (</span><span class="default">$iterator </span><span class="keyword">as </span><span class="default">$file</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="default">$file</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>Not to mention the fact that $file will be an SplFileInfo class, so you can do powerful stuff really easily:<br><br><span class="default">&lt;?php<br><br>$size </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>foreach (</span><span class="default">$iterator </span><span class="keyword">as </span><span class="default">$file</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">isFile</span><span class="keyword">()) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">getPathname</span><span class="keyword">(), </span><span class="default">27</span><span class="keyword">) . </span><span class="string">&quot;: &quot; </span><span class="keyword">. </span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">getSize</span><span class="keyword">() . </span><span class="string">&quot; B; modified &quot; </span><span class="keyword">. </span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;Y-m-d&quot;</span><span class="keyword">, </span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">getMTime</span><span class="keyword">()) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">+= </span><span class="default">$file</span><span class="keyword">-&gt;</span><span class="default">getSize</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>}<br><br>echo </span><span class="string">&quot;\nTotal file size: &quot;</span><span class="keyword">, </span><span class="default">$size</span><span class="keyword">, </span><span class="string">&quot; bytes\n&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>\Luna\luna.msstyles: 4190352 B; modified 2008-04-13<br>\Luna\Shell\Homestead\shellstyle.dll: 362496 B; modified 2006-02-28<br>\Luna\Shell\Metallic\shellstyle.dll: 362496 B; modified 2006-02-28<br>\Luna\Shell\NormalColor\shellstyle.dll: 361472 B; modified 2006-02-28<br>\Luna.theme: 1222 B; modified 2006-02-28<br>\Windows Classic.theme: 3025 B; modified 2006-02-28<br><br>Total file size: 5281063 bytes</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that glob(&apos;*&apos;) ignores all &apos;hidden&apos; files by default. This means it does not return files that start with a dot (e.g. &quot;.file&quot;).
<br>If you want to match those files too, you can use &quot;{,.}*&quot; as the pattern with the GLOB_BRACE flag.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// Search for all files that match .* or *
<br></span><span class="default">$files </span><span class="keyword">= </span><span class="default">glob</span><span class="keyword">(</span><span class="string">&apos;{,.}*&apos;</span><span class="keyword">, </span><span class="default">GLOB_BRACE</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Note: This also returns the directory special entries . and ..</span>
</div>
  

#


<div class="phpcode"><span class="html">
glob is case sensitive, even on Windows systems.<br><br>It does support character classes though, so a case insensitive version of<br><span class="default">&lt;?php glob</span><span class="keyword">(</span><span class="string">&apos;my/dir/*.csv&apos;</span><span class="keyword">) </span><span class="default">?&gt;<br></span><br>could be written as<br><span class="default">&lt;?php glob</span><span class="keyword">(</span><span class="string">&apos;my/dir/*.[cC][sS][vV]&apos;</span><span class="keyword">) </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
glob() isn&apos;t limited to one directory:<br><br><span class="default">&lt;?php<br>$results</span><span class="keyword">=</span><span class="default">glob</span><span class="keyword">(</span><span class="string">&quot;{includes/*.php,core/*.php}&quot;</span><span class="keyword">,</span><span class="default">GLOB_BRACE</span><span class="keyword">);<br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">,</span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$results</span><span class="keyword">,</span><span class="default">true</span><span class="keyword">),</span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Just be careful when using GLOB_BRACE regarding spaces around the comma:<br>{includes/*.php,core/*.php} works as expected, but<br>{includes/*.php, core/*.php} with a leading space, will only match the former as expected but not the latter<br>unless you have a directory named &quot; core&quot; on your machine with a leading space.<br>PHP can create such directories quite easily like so:<br>mkdir(&quot; core&quot;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that in case you are using braces with glob you might retrieve duplicated entries for files that matche more than one item :<br><br><span class="default">&lt;?php<br><br>$a </span><span class="keyword">= </span><span class="default">glob</span><span class="keyword">(</span><span class="string">&apos;/path/*{foo,bar}.dat&apos;</span><span class="keyword">,</span><span class="default">GLOB_BRACE</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Result : <br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; /path/file_foo.dat<br>&#xA0; &#xA0; [1] =&gt; /path/file_foobar.dat<br>&#xA0; &#xA0; [2] =&gt; /path/file_foobar.dat<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Don&apos;t use glob() if you try to list files in a directory where very much files are stored (&gt;100.000). You get an &quot;Allowed memory size of XYZ bytes exhausted ...&quot; error.<br>You may try to increase the memory_limit variable in php.ini. Mine has 128MB set and the script will still reach this limit while glob()ing over 500.000 files.<br><br>The more stable way is to use readdir() on very large numbers of files:<br><span class="default">&lt;?php<br></span><span class="comment">// code snippet<br></span><span class="keyword">if (</span><span class="default">$handle </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="default">$path</span><span class="keyword">)) {<br>&#xA0; &#xA0; while (</span><span class="default">false </span><span class="keyword">!== (</span><span class="default">$file </span><span class="keyword">= </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">))) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// do something with the file<br>&#xA0; &#xA0; &#xA0; &#xA0; // note that &apos;.&apos; and &apos;..&apos; is returned even<br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; </span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="keyword">if ( ! </span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;glob_recursive&apos;</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; </span><span class="comment">// Does not support flag GLOB_BRACE<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">glob_recursive</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$flags </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$files </span><span class="keyword">= </span><span class="default">glob</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$flags</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">glob</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">).</span><span class="string">&apos;/*&apos;</span><span class="keyword">, </span><span class="default">GLOB_ONLYDIR</span><span class="keyword">|</span><span class="default">GLOB_NOSORT</span><span class="keyword">) as </span><span class="default">$dir</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$files </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$files</span><span class="keyword">, </span><span class="default">glob_recursive</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">.</span><span class="string">&apos;/&apos;</span><span class="keyword">.</span><span class="default">basename</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">), </span><span class="default">$flags</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$files</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.glob.php)

**[To root](/README.md)**