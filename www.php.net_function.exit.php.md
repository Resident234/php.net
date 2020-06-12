# exit




<div class="phpcode"><span class="html">
If you want to avoid calling exit() in FastCGI as per the comments below, but really, positively want to exit cleanly from nested function call or include, consider doing it the Python way:<br><br>define an exception named `SystemExit&apos;, throw it instead of calling exit() and catch it in index.php with an empty handler to finish script execution cleanly.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// file: index.php<br></span><span class="keyword">class </span><span class="default">SystemExit </span><span class="keyword">extends </span><span class="default">Exception </span><span class="keyword">{}<br>try {<br>&#xA0;&#xA0; </span><span class="comment">/* code code */<br></span><span class="keyword">}<br>catch (</span><span class="default">SystemExit $e</span><span class="keyword">) { </span><span class="comment">/* do nothing */ </span><span class="keyword">}<br></span><span class="comment">// end of file: index.php<br><br>// some deeply nested function or .php file&#xA0; &#xA0; <br><br></span><span class="keyword">if (</span><span class="default">SOME_EXIT_CONDITION</span><span class="keyword">)<br>&#xA0;&#xA0; throw new </span><span class="default">SystemExit</span><span class="keyword">(); </span><span class="comment">// instead of exit()<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
jbezorg at gmail proposed the following:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">if(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;SCRIPT_FILENAME&apos;</span><span class="keyword">] == </span><span class="default">__FILE__ </span><span class="keyword">)<br>&#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Location: /&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>After sending the `Location:&apos; header PHP _will_ continue parsing, and all code below the header() call will still be executed.&#xA0; So instead use:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">if(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;SCRIPT_FILENAME&apos;</span><span class="keyword">] == </span><span class="default">__FILE__</span><span class="keyword">)<br>{<br>&#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Location: /&apos;</span><span class="keyword">);<br>&#xA0; exit;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Don&apos;t use the&#xA0; exit() function in the auto prepend file with fastcgi (linux/bsd os).<br>It has the effect of leaving opened files with for result at least a nice&#xA0; &quot;Too many open files&#xA0; ...&quot; error.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To rich dot lovely at klikzltd dot co dot uk:<br><br>Using a &quot;@&quot; before header() to suppress its error, and relying on the &quot;headers already sent&quot; error seems to me a very bad idea while building any serious website.<br><br>This is *not* a clean way to prevent a file from being called directly. At least this is not a secure method, as you rely on the presence of an exception sent by the parser at runtime.<br><br>I recommend using a more common way as defining a constant or assigning a variable with any value, and checking for its presence in the included script, like:<br><br>in index.php:<br><span class="default">&lt;?php<br>define </span><span class="keyword">(</span><span class="string">&apos;INDEX&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>in your included file:<br><span class="default">&lt;?php<br></span><span class="keyword">if (!</span><span class="default">defined</span><span class="keyword">(</span><span class="string">&apos;INDEX&apos;</span><span class="keyword">)) {<br>&#xA0;&#xA0; die(</span><span class="string">&apos;You cannot call this script directly !&apos;</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>BR.<br><br>Ninj</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.exit.php)

**[⬆ to root](/)**