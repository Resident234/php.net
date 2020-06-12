# rtrim




<div class="phpcode"><span class="html">
I have an obsessive love for php&apos;s array functions given how extremely easy they&apos;ve made complex string handling for me in various situations... so, have another string-rtrim() variant:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">strrtrim</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">, </span><span class="default">$strip</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">// break message apart by strip string
<br>&#xA0; &#xA0; </span><span class="default">$lines </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$strip</span><span class="keyword">, </span><span class="default">$message</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$last&#xA0; </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// pop off empty strings at the end
<br>&#xA0; &#xA0; </span><span class="keyword">do {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$last </span><span class="keyword">= </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">);
<br>&#xA0; &#xA0; } while (empty(</span><span class="default">$last</span><span class="keyword">) &amp;&amp; (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">)));
<br>&#xA0; &#xA0; </span><span class="comment">// re-assemble what remains
<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">implode</span><span class="keyword">(</span><span class="default">$strip</span><span class="keyword">, </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">, array(</span><span class="default">$last</span><span class="keyword">)));
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Astonishingly, something I didn&apos;t expect, but: It completely compares to harmor&apos;s rstrtrim below, execution time wise. o_o Whee!</span>
</div>
  

#


<div class="phpcode"><span class="html">
True, the Perl chomp() will only trim newline characters. There is, however, the Perl chop() function which is pretty much identical to the PHP rtrim()
<br>
<br>---
<br>
<br>Here&apos;s a quick way to recursively trim every element of an array, useful after the file() function :
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment"># Reads /etc/passwd file an trims newlines on each entry
<br></span><span class="default">$aFileContent </span><span class="keyword">= </span><span class="default">file</span><span class="keyword">(</span><span class="string">&quot;/etc/passwd&quot;</span><span class="keyword">);
<br>foreach (</span><span class="default">$aFileContent </span><span class="keyword">as </span><span class="default">$sKey </span><span class="keyword">=&gt; </span><span class="default">$sValue</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$aFileContent</span><span class="keyword">[</span><span class="default">$sKey</span><span class="keyword">] = </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$sValue</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$aFileContent</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This shows how rtrim works when using the optional charlist parameter:<br>rtrim reads a character, one at a time, from the optional charlist parameter and compares it to the end of the str string. If the characters match, it trims it off and starts over again, looking at the &quot;new&quot; last character in the str string and compares it to the first character in the charlist again. If the characters do not match, it moves to the next character in the charlist parameter comparing once again. It continues until the charlist parameter has been completely processed, one at a time, and the str string no longer contains any matches. The newly &quot;rtrimmed&quot; string is returned.<br><span class="default">&lt;?php<br>&#xA0; </span><span class="comment">// Example 1:<br>&#xA0; </span><span class="default">rtrim</span><span class="keyword">(</span><span class="string">&apos;This is a short short sentence&apos;</span><span class="keyword">, </span><span class="string">&apos;short sentence&apos;</span><span class="keyword">);<br>&#xA0; </span><span class="comment">// returns &apos;This is a&apos;<br>&#xA0; // If you were expecting the result to be &apos;This is a short &apos;,<br>&#xA0; // then you&apos;re wrong; the exact string, &apos;short sentence&apos;,<br>&#xA0; // isn&apos;t matched.&#xA0; Remember, character-by-character comparison!<br>&#xA0; // Example 2:<br>&#xA0; </span><span class="default">rtrim</span><span class="keyword">(</span><span class="string">&apos;This is a short short sentence&apos;</span><span class="keyword">, </span><span class="string">&apos;cents&apos;</span><span class="keyword">);<br>&#xA0; </span><span class="comment">// returns &apos;This is a short short &apos;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
On the recurring subject of string-stripping instead of character-stripping rtrim() implementations... the simplest (with a caveat) is probably the basename() function. It has a second parameter that functions as a right-trim using whole strings:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">echo </span><span class="default">basename</span><span class="keyword">(</span><span class="string">&apos;MooFoo&apos;</span><span class="keyword">, </span><span class="string">&apos;Foo&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>...outputs &apos;Moo&apos;.<br><br>Since it also strips anything that looks like a directory, it&apos;s not quite identical with hacking a string off the end:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">echo </span><span class="default">basename</span><span class="keyword">(</span><span class="string">&apos;Zoo/MooFoo&apos;</span><span class="keyword">, </span><span class="string">&apos;Foo&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>...still outputs &apos;Moo&apos;.<br><br>But sometimes it gets the job done.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I needed a way to trim all white space and then a few chosen strings from the end of a string.&#xA0; So I wrote this class to reuse when stuff needs to be trimmed.&#xA0; <br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">cleaner </span><span class="keyword">{<br><br>function </span><span class="default">cleaner </span><span class="keyword">(</span><span class="default">$cuts</span><span class="keyword">,</span><span class="default">$pinfo</span><span class="keyword">) {<br></span><span class="default">$ucut </span><span class="keyword">= </span><span class="string">&quot;0&quot;</span><span class="keyword">;<br></span><span class="default">$lcut </span><span class="keyword">= </span><span class="string">&quot;0&quot;</span><span class="keyword">;<br>while (</span><span class="default">$cuts</span><span class="keyword">[</span><span class="default">$ucut</span><span class="keyword">]) {<br></span><span class="default">$lcut</span><span class="keyword">++;<br></span><span class="default">$ucut</span><span class="keyword">++;<br>}<br></span><span class="default">$lcut </span><span class="keyword">= </span><span class="default">$lcut </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">;<br></span><span class="default">$ucut </span><span class="keyword">= </span><span class="string">&quot;0&quot;</span><span class="keyword">;<br></span><span class="default">$rcut </span><span class="keyword">= </span><span class="string">&quot;0&quot;</span><span class="keyword">;<br></span><span class="default">$wiy </span><span class="keyword">= </span><span class="string">&quot;start&quot;</span><span class="keyword">;<br><br>while (</span><span class="default">$wiy</span><span class="keyword">) {<br><br>if (</span><span class="default">$so</span><span class="keyword">) {<br></span><span class="default">$ucut </span><span class="keyword">= </span><span class="string">&quot;0&quot;</span><span class="keyword">;<br></span><span class="default">$rcut </span><span class="keyword">= </span><span class="string">&quot;0&quot;</span><span class="keyword">;<br>unset(</span><span class="default">$so</span><span class="keyword">);<br>}<br><br>if (!</span><span class="default">$cuts</span><span class="keyword">[</span><span class="default">$ucut</span><span class="keyword">]) {<br></span><span class="default">$so </span><span class="keyword">= </span><span class="string">&quot;restart&quot;</span><span class="keyword">;<br>} else {<br></span><span class="default">$pinfo </span><span class="keyword">= </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$pinfo</span><span class="keyword">);<br></span><span class="default">$bpinfol </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$pinfo</span><span class="keyword">);<br></span><span class="default">$tcut </span><span class="keyword">= </span><span class="default">$cuts</span><span class="keyword">[</span><span class="default">$ucut</span><span class="keyword">];<br></span><span class="default">$pinfo </span><span class="keyword">= </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$pinfo</span><span class="keyword">,</span><span class="string">&quot;</span><span class="default">$tcut</span><span class="string">&quot;</span><span class="keyword">);<br></span><span class="default">$pinfol </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$pinfo</span><span class="keyword">);<br><br>&#xA0; &#xA0; if (</span><span class="default">$bpinfol </span><span class="keyword">== </span><span class="default">$pinfol</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$rcut</span><span class="keyword">++;<br>&#xA0; &#xA0; if (</span><span class="default">$rcut </span><span class="keyword">== </span><span class="default">$lcut</span><span class="keyword">) {<br>&#xA0; &#xA0; unset(</span><span class="default">$wiy</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">$ucut</span><span class="keyword">++;<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; </span><span class="default">$so </span><span class="keyword">= </span><span class="string">&quot;restart&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br>}<br><br></span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">cleaner </span><span class="keyword">= </span><span class="default">$pinfo</span><span class="keyword">;<br>}<br><br>}<br><br></span><span class="default">$pinfo </span><span class="keyword">= </span><span class="string">&quot;Well... I&apos;m really bored...&lt;br /&gt;&lt;br&gt;&amp;nbsp;&#xA0; &#xA0; \n\t&amp;nbsp;&lt;br&gt;&lt;br /&gt;&lt;br&gt;&amp;nbsp;&#xA0; &#xA0; \r\r&amp;nbsp;&lt;br&gt;\r&lt;br /&gt;&lt;br&gt;\r&amp;nbsp;&#xA0; &#xA0; &amp;nbsp;\n&lt;br&gt;&#xA0; &#xA0; &#xA0; &lt;br /&gt;\t&quot;</span><span class="keyword">;<br><br></span><span class="default">$cuts </span><span class="keyword">= array(</span><span class="string">&apos;\n&apos;</span><span class="keyword">,</span><span class="string">&apos;\r&apos;</span><span class="keyword">,</span><span class="string">&apos;\t&apos;</span><span class="keyword">,</span><span class="string">&apos; &apos;</span><span class="keyword">,</span><span class="string">&apos; &apos;</span><span class="keyword">,</span><span class="string">&apos;&amp;nbsp;&apos;</span><span class="keyword">,</span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">,</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">,</span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">);<br><br></span><span class="default">$pinfo </span><span class="keyword">= new </span><span class="default">cleaner</span><span class="keyword">(</span><span class="default">$cuts</span><span class="keyword">,</span><span class="default">$pinfo</span><span class="keyword">);<br></span><span class="default">$pinfo </span><span class="keyword">= </span><span class="default">$pinfo</span><span class="keyword">-&gt;</span><span class="default">cleaner</span><span class="keyword">;<br><br>print </span><span class="default">$pinfo</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>That class will take any string that you put in the $cust array and remove it from the end of the $pinfo string.&#xA0; It&apos;s useful for cleaning up comments, articles, or mail that users post to your site, making it so there&apos;s no extra blank space or blank lines.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.rtrim.php)

**[⬆ to root](/)**