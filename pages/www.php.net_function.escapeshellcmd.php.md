# escapeshellcmd




<div class="phpcode"><span class="html">
There is a quirk to be aware of regarding use of echo. If you have a command which you want to execute which takes input from STDIN, you would normally do:
<br>
<br><span class="default">&lt;?php $output </span><span class="keyword">= </span><span class="default">shell_exec</span><span class="keyword">(</span><span class="string">&quot;echo </span><span class="default">$input</span><span class="string"> | /the/command&quot;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>Unfortunately, this is a *bad idea* and will make your script unportable, providing a very hard-to-trace bug on some systems. Depending on how the server is set up, /bin/sh will either call /bin/bash or /bin/dash, and these have very different versions of echo. Never use echo; use printf instead which is consistent. How do you escape for printf? Do this:
<br>
<br><span class="default">&lt;?php
<br>$input </span><span class="keyword">= </span><span class="string">&apos;string to be passed *exactly* to the command&apos;</span><span class="keyword">;
<br></span><span class="comment">//Escape only what is needed to get by PHP&apos;s parser; we want
<br>//the string data PHP is holding in its buffer to be passed
<br>//exactly to stdin buffer of the command.
<br></span><span class="default">$cmd </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&apos;\\&apos;</span><span class="keyword">, </span><span class="string">&apos;%&apos;</span><span class="keyword">), array(</span><span class="string">&apos;\\\\&apos;</span><span class="keyword">, </span><span class="string">&apos;%%&apos;</span><span class="keyword">), </span><span class="default">$input</span><span class="keyword">);
<br></span><span class="default">$cmd </span><span class="keyword">= </span><span class="default">escapeshellarg</span><span class="keyword">(</span><span class="default">$cmd</span><span class="keyword">);
<br>
<br></span><span class="default">$output </span><span class="keyword">= </span><span class="default">shell_exec</span><span class="keyword">(</span><span class="string">&quot;printf </span><span class="default">$cmd</span><span class="string"> | /path/to/command&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>For the paranoid, this torture test verifies that both shell escaping and printf&apos;s own escaping are handled correctly. Use with confidence!
<br>
<br><span class="default">&lt;?php
<br>
<br>$test </span><span class="keyword">= </span><span class="string">&apos;stuff bash interprets, space: # &amp; ; ` | * ? ~ &lt; &gt; ^ ( ) [ ] { } $ \ \x0A \xFF. \&apos; &quot; %&apos;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;stuff bash interprets, no space: #&amp;;`|*?~&lt;&gt;^()[]{}$\\x0A\xFF.\&apos;\&quot;%&apos;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;stuff bash interprets, with leading backslash: \# \&amp; \; \` \| \* \? \~ \&lt; \&gt; \^ \( \) \[ \] \{ \} \$ \\\ \\\x0A \\\xFF. \\\&apos; \&quot; \%&apos;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;printf codes: % \ (used to form %.0#-*+d, or \\ \a \b \f \n \r \t \v \&quot; \? \062 \0062 \x032 \u0032 and \U00000032)&apos;</span><span class="keyword">;
<br>
<br>echo </span><span class="string">&quot;These are the strings we are testing with:&quot;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">.</span><span class="default">$test</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">;
<br></span><span class="default">$cmd </span><span class="keyword">= </span><span class="default">$test</span><span class="keyword">;
<br></span><span class="default">$cmd </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&apos;\\&apos;</span><span class="keyword">, </span><span class="string">&apos;%&apos;</span><span class="keyword">), array(</span><span class="string">&apos;\\\\&apos;</span><span class="keyword">, </span><span class="string">&apos;%%&apos;</span><span class="keyword">), </span><span class="default">$test</span><span class="keyword">);
<br></span><span class="default">$cmd </span><span class="keyword">= </span><span class="default">escapeshellarg</span><span class="keyword">(</span><span class="default">$cmd</span><span class="keyword">);
<br>
<br>echo </span><span class="default">PHP_EOL</span><span class="keyword">.</span><span class="string">&quot;This is the output using the escaping mechanism given:&quot;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">;
<br>echo `</span><span class="string">printf </span><span class="default">$cmd</span><span class="string"> | cat</span><span class="keyword">`.</span><span class="default">PHP_EOL</span><span class="keyword">;
<br>
<br>echo </span><span class="default">PHP_EOL</span><span class="keyword">.</span><span class="string">&quot;They should match exactly&quot;</span><span class="keyword">.</span><span class="default">PHP_EOL</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.escapeshellcmd.php)

**[To root](/README.md)**