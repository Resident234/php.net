# parse_ini_file




<div class="phpcode"><span class="html">
I use the following syntax to secure my config.ini.php file:<br><br>;<span class="default">&lt;?php<br></span><span class="keyword">;die(); </span><span class="comment">// For further security<br></span><span class="keyword">;</span><span class="comment">/*<br><br>[category]<br>name=&quot;value&quot;<br><br>;*/<br><br></span><span class="keyword">;</span><span class="default">?&gt;<br></span><br>Works like a charm and is both: A valid PHP File and a valid ini-File ;)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Undocumented feature!<br><br>Using ${...} as a value will look to<br>1) an INI setting, or<br>2) an environment variable<br><br>For example,<br><br><span class="default">&lt;?php<br><br>print_r</span><span class="keyword">(</span><span class="default">parse_ini_string</span><span class="keyword">(</span><span class="string">&apos;<br>php_ext_dir = ${extension_dir}<br>operating_system = ${OS}<br>&apos;</span><span class="keyword">));<br><br></span><span class="default">?&gt;<br></span><br>Array<br>(<br>&#xA0; &#xA0; [php_ext_dir] =&gt; ./ext/<br>&#xA0; &#xA0; [operating_system] =&gt; Windows_NT<br>)<br><br>Present in PHP 5.3.2, likely in 5.x, maybe even earlier too.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If your configuration file holds any sensitive information (such as database login details), remember NOT to place it within your document root folder! A common mistake is to replace config.inc.php files, which are formatted in PHP:<br><span class="default">&lt;?php<br>$database</span><span class="keyword">[</span><span class="string">&apos;host&apos;</span><span class="keyword">] = </span><span class="string">&apos;localhost&apos;</span><span class="keyword">;<br></span><span class="comment">// etc...<br></span><span class="default">?&gt;<br></span><br>With config.ini files which are written in plain text:<br>[database]<br>host = localhost<br><br>The file config.ini can be read by anyone who knows where it&apos;s located, if it&apos;s under your document root folder. Remember to place it above!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a quick parse_ini_file wrapper to add extend support to save typing and redundancy.<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Parses INI file adding extends functionality via &quot;:base&quot; postfix on namespace.<br>&#xA0; &#xA0;&#xA0; *<br>&#xA0; &#xA0;&#xA0; * @param string $filename<br>&#xA0; &#xA0;&#xA0; * @return array<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">parse_ini_file_extended</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$p_ini </span><span class="keyword">= </span><span class="default">parse_ini_file</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$config </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$p_ini </span><span class="keyword">as </span><span class="default">$namespace </span><span class="keyword">=&gt; </span><span class="default">$properties</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$name</span><span class="keyword">, </span><span class="default">$extends</span><span class="keyword">) = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;:&apos;</span><span class="keyword">, </span><span class="default">$namespace</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$name </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$extends </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$extends</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// create namespace if necessary<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(!isset(</span><span class="default">$config</span><span class="keyword">[</span><span class="default">$name</span><span class="keyword">])) </span><span class="default">$config</span><span class="keyword">[</span><span class="default">$name</span><span class="keyword">] = array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// inherit base namespace<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(isset(</span><span class="default">$p_ini</span><span class="keyword">[</span><span class="default">$extends</span><span class="keyword">])){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$p_ini</span><span class="keyword">[</span><span class="default">$extends</span><span class="keyword">] as </span><span class="default">$prop </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$config</span><span class="keyword">[</span><span class="default">$name</span><span class="keyword">][</span><span class="default">$prop</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// overwrite / set current namespace values<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">foreach(</span><span class="default">$properties </span><span class="keyword">as </span><span class="default">$prop </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$config</span><span class="keyword">[</span><span class="default">$name</span><span class="keyword">][</span><span class="default">$prop</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$config</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Treats this ini:<br><span class="default">&lt;?php <br></span><span class="comment">/*<br>[base]<br>host=localhost<br>user=testuser<br>pass=testpass<br>database=default<br><br>[users:base]<br>database=users<br><br>[archive : base]<br>database=archive<br>*/<br></span><span class="default">?&gt;<br></span>As if it were like this:<br><span class="default">&lt;?php<br></span><span class="comment">/*<br>[base]<br>host=localhost<br>user=testuser<br>pass=testpass<br>database=default<br><br>[users:base]<br>host=localhost<br>user=testuser<br>pass=testpass<br>database=users<br><br>[archive : base]<br>host=localhost<br>user=testuser<br>pass=testpass<br>database=archive<br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
And for the extra-paranoid like myself, add a rule into your httpd.conf file so that *.ini (or *.inc) in my case can&apos;t be sent to a browser:<br><br>&lt;Files *.inc&gt;&#xA0; <br>&#xA0; &#xA0; Order deny,allow<br>&#xA0; &#xA0; Deny from all<br>&lt;/Files&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
To those who were like me looking if this could be used to create an array out of commandline output I offer you the function below (I used it to parse mplayer output).<br><br>If you want it behave exactly the same as parse_ini_file you&apos;ll obviously have to add some code to feed the different sections to this one. Hope it&apos;s of help to someone!<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * The return is very similar to that of parse_ini_file, but this works off files<br> * <br> * Below is an example of what it does, where the first<br> * value is what you&apos;d normally want to do, and the second and third things that might<br> * happen and in case it does it&apos;s good to know what is going on.<br> * <br> * $anArray = array( &apos;default=theValue&apos;, &apos;setting=&apos;, &apos;something=value=value&apos; );<br> * explodeExplode( &apos;=&apos;, $anArray );<br> * <br> * the return will be <br> * array( &apos;default&apos; =&gt; &apos;theValue&apos;, &apos;setting&apos; =&gt; &apos;&apos;, &apos;something&apos; =&gt; &apos;value=value&apos; );<br> * <br> * So the oddities here are, text after the second $string occurence dissapearing<br> * and empty values resulting in an empty string.<br> * <br> * @return $returnArray array array( &apos;setting&apos; =&gt; &apos;value&apos; )<br> * @param $string Object<br> * @param $array Object<br> */<br></span><span class="keyword">function </span><span class="default">explodeExplode</span><span class="keyword">( </span><span class="default">$string</span><span class="keyword">, </span><span class="default">$array </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$returnArray </span><span class="keyword">= array();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; foreach( </span><span class="default">$array </span><span class="keyword">as </span><span class="default">$arrayValue </span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tmpArray </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">( </span><span class="default">$string</span><span class="keyword">, </span><span class="default">$arrayValue </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if( </span><span class="default">count</span><span class="keyword">( </span><span class="default">$tmpArray </span><span class="keyword">) == </span><span class="default">1 </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$returnArray</span><span class="keyword">[</span><span class="default">$tmpArray</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]] = </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; else if( </span><span class="default">count</span><span class="keyword">( </span><span class="default">$tmpArray </span><span class="keyword">) == </span><span class="default">2 </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$returnArray</span><span class="keyword">[</span><span class="default">$tmpArray</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]] = </span><span class="default">$tmpArray</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; else if( </span><span class="default">count</span><span class="keyword">( </span><span class="default">$tmpArray </span><span class="keyword">) &gt; </span><span class="default">2 </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$implodeBack </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$firstLoop&#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach( </span><span class="default">$tmpArray </span><span class="keyword">as </span><span class="default">$tmpValue </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if( </span><span class="default">$firstLoop </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$firstLoop </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$implodeBack</span><span class="keyword">[] = </span><span class="default">$tmpValue</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$implodeBack </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$returnArray</span><span class="keyword">[</span><span class="default">$tmpArray</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]] = </span><span class="default">implode</span><span class="keyword">( </span><span class="string">&apos;=&apos;</span><span class="keyword">, </span><span class="default">$implodeBack </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="default">$returnArray</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-ini-file.php)

**[To root](/README.md)**