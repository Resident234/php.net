# Interactive shell




<div class="phpcode"><span class="html">
Interactive Shell and Interactive Mode are not the same thing, despite the similar names and functionality.<br><br>If you type &apos;php -a&apos; and get a response of &apos;Interactive Shell&apos; followed by a &apos;php&gt;&apos; prompt, you have interactive shell available (PHP was compiled with readline support). If instead you get a response of &apos;Interactive mode enabled&apos;, you DO NOT have interactive shell available and this article does not apply to you.<br><br>You can also check &apos;php -m&apos; and see if readline is listed in the output - if not, you don&apos;t have interactive shell.<br><br>Interactive mode is essentially like running php with stdin as the file input. You just type code, and when you&apos;re done (Ctrl-D), php executes whatever you typed as if it were a normal PHP (PHTML) file - hence you start in interactive mode with &apos;&lt;?php&apos; in order to execute code.<br><br>Interactive shell evaluates every expression as you complete it (with ; or }), reports errors without terminating execution, and supports standard shell functionality via readline (history, tab completion, etc). It&apos;s an enhanced version of interactive mode that is ONLY available if you have the required libraries, and is an actual PHP shell that interprets everything you type as PHP code - using &apos;&lt;?php&apos; will cause a parse error.<br><br>Finally, if you&apos;re running on Windows, you&apos;re probably screwed. From what I&apos;m seeing in other comments here, you don&apos;t have readline, and without readline there is no interactive shell.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In Windows, press Enter after your ending PHP tag and then hit Ctrl-Z to denote the end-of-file:
<br>
<br>C:\&gt;php -a
<br>Interactive mode enabled
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">echo </span><span class="string">&quot;Hello, world!&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>^Z
<br>Hello, world!
<br>
<br>You can use the up and down arrows in interactive mode to recall previous code you ran.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems the interactive shell cannot be made to work in WIN environments at the moment.&#xA0; <br><br>Using &quot;php://stdin&quot;, it shouldn&apos;t be too difficult to roll your own.&#xA0; You can partially mimic the shell by calling this simple script (Note: Window&apos;s cmd already has an input history calling feature using the up/down keys, and that functionality will still be available during execution here):<br><br><span class="default">&lt;?php<br><br>$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;php://stdin&quot;</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">);<br></span><span class="default">$in </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>while(</span><span class="default">$in </span><span class="keyword">!= </span><span class="string">&quot;quit&quot;</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;php&gt; &quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$in</span><span class="keyword">=</span><span class="default">trim</span><span class="keyword">(</span><span class="default">fgets</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">));<br>&#xA0; &#xA0; eval (</span><span class="default">$in</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br></span><span class="default">?&gt;<br></span><br>Replace &apos;eval&apos; with code to parse the input string, validate it using is_callable and other variable handling functions, catch fatal errors before they happen, allow line-by-line function defining, etc.&#xA0; Though Readline is not available in Windows, for more tips and examples for workarounds, see <a href="http://www.php.net/manual/en/ref.readline.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/ref.readline.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
For use interactive mode enabled on GNU/Linux on distros Debian/Ubuntu/LinuxMint you must install &quot;php*-cli&quot; and &quot;php*-readline&quot; packages from official repository.<br>Example:<br> &gt;$sudo aptitude install php5-cli php5-readline<br><br>After that you can use interactive mode.<br>Example:<br>~ $ php -a<br>Interactive mode enabled<br><br>php &gt;echo &quot;hola mundo!\n&quot;;<br>hola mundo!<br>php &gt;<br><br>I hope somebody help it!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a few more notes to add...<br><br>1) Hitting return does literally mean &quot;execute this command&quot;.&#xA0; Semicolon to note end of line is still required.&#xA0; Meaning, doing the following will produce a parse error:<br><br>php &gt; print &quot;test&quot;<br>php &gt; print &quot;asdf&quot;;<br><br>Whereas doing the following is just fine:<br><br>php &gt; print &quot;test&quot;<br>php &gt; .&quot;asdf&quot;;<br><br>2) Fatal errors may eject you from the shell:<br><br>name@local:~$ php -a<br>php &gt; asdf();<br><br>Fatal Error: call to undefined function...<br>name@local:~$<br><br>3) User defined functions are not saved in history from shell session to shell session.<br><br>4) Should be obvious, but to quit the shell, just type &quot;quit&quot; at the php prompt.<br><br>5) In a sense, the shell interaction can be thought of as linearly following a regular php file, except it&apos;s live and dynamic.&#xA0; If you define a function that you&apos;ve already defined earlier in your current shell, you will receive a fatal &quot;function already defined&quot; error only upon entering that closing bracket.&#xA0; And, although &quot;including&quot; a toolset of custom functions or a couple of script addon php files is rather handy, should you edit those files and wish to &quot;reinclude&quot; it again, you&apos;ll cause a fatal &quot;function x already defined&quot; error.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.interactive.php)

**[To root](/README.md)**