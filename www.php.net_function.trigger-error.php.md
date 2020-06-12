# trigger_error




<div class="phpcode"><span class="html">
the idea is never to give out file names, line numbers, and cryptic codes to the user. Use trigger_error() after you used set_error_handler() to register your own callback function which either logs or emails the error codes to you, and echo a simple friendly message to the user.
<br>
<br>And turn on a more verbose error handler function when you need to debug your scripts. In my init.php scripts I always have:
<br>
<br>if (_DEBUG_) {
<br>&#xA0; &#xA0; set_error_handler (&apos;debug_error_handler&apos;);
<br>}
<br>else {
<br>&#xA0; &#xA0; set_error_handler (&apos;nice_error_handler&apos;);
<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
trigger_error always reports the line and file that trigger_error was called on. Which isn&apos;t very useful.
<br>
<br>eg:
<br>
<br>main.php:
<br><span class="default">&lt;?php
<br></span><span class="keyword">include(</span><span class="string">&apos;functions.php&apos;</span><span class="keyword">);
<br></span><span class="default">$x </span><span class="keyword">= </span><span class="string">&apos;test&apos;</span><span class="keyword">;
<br></span><span class="default">doFunction</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>functions.php:
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">doFunction</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) {
<br>if(</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)) {
<br> </span><span class="comment">/* do some stuff*/
<br></span><span class="keyword">} else {
<br> </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="string">&apos;var must be numeric&apos;</span><span class="keyword">);
<br>}
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>will output &quot;Notice: var must be numeric in functions.php on line 6&quot;
<br>whereas &quot;Notice: var must be numeric in main.php on line 4&quot; would be more useful
<br>
<br>here&apos;s a function to do that:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">error</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">, </span><span class="default">$level</span><span class="keyword">=</span><span class="default">E_USER_NOTICE</span><span class="keyword">) {
<br></span><span class="default">$caller </span><span class="keyword">= </span><span class="default">next</span><span class="keyword">(</span><span class="default">debug_backtrace</span><span class="keyword">());
<br></span><span class="default">trigger_error</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">.</span><span class="string">&apos; in &lt;strong&gt;&apos;</span><span class="keyword">.</span><span class="default">$caller</span><span class="keyword">[</span><span class="string">&apos;function&apos;</span><span class="keyword">].</span><span class="string">&apos;&lt;/strong&gt; called from &lt;strong&gt;&apos;</span><span class="keyword">.</span><span class="default">$caller</span><span class="keyword">[</span><span class="string">&apos;file&apos;</span><span class="keyword">].</span><span class="string">&apos;&lt;/strong&gt; on line &lt;strong&gt;&apos;</span><span class="keyword">.</span><span class="default">$caller</span><span class="keyword">[</span><span class="string">&apos;line&apos;</span><span class="keyword">].</span><span class="string">&apos;&lt;/strong&gt;&apos;</span><span class="keyword">.</span><span class="string">&quot;\n&lt;br /&gt;error handler&quot;</span><span class="keyword">, </span><span class="default">$level</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>So now in our example:
<br>
<br>main.php:
<br><span class="default">&lt;?php
<br></span><span class="keyword">include(</span><span class="string">&apos;functions.php&apos;</span><span class="keyword">);
<br></span><span class="default">$x </span><span class="keyword">= </span><span class="string">&apos;test&apos;</span><span class="keyword">;
<br></span><span class="default">doFunction</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>functions.php:
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">doFunction</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) {
<br>&#xA0; &#xA0; if(</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">/* do some stuff*/
<br>&#xA0; &#xA0; </span><span class="keyword">} else {
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">error</span><span class="keyword">(</span><span class="string">&apos;var must be numeric&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>function </span><span class="default">error</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">, </span><span class="default">$level</span><span class="keyword">=</span><span class="default">E_USER_NOTICE</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$caller </span><span class="keyword">= </span><span class="default">next</span><span class="keyword">(</span><span class="default">debug_backtrace</span><span class="keyword">());
<br>&#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">.</span><span class="string">&apos; in &lt;strong&gt;&apos;</span><span class="keyword">.</span><span class="default">$caller</span><span class="keyword">[</span><span class="string">&apos;function&apos;</span><span class="keyword">].</span><span class="string">&apos;&lt;/strong&gt; called from &lt;strong&gt;&apos;</span><span class="keyword">.</span><span class="default">$caller</span><span class="keyword">[</span><span class="string">&apos;file&apos;</span><span class="keyword">].</span><span class="string">&apos;&lt;/strong&gt; on line &lt;strong&gt;&apos;</span><span class="keyword">.</span><span class="default">$caller</span><span class="keyword">[</span><span class="string">&apos;line&apos;</span><span class="keyword">].</span><span class="string">&apos;&lt;/strong&gt;&apos;</span><span class="keyword">.</span><span class="string">&quot;\n&lt;br /&gt;error handler&quot;</span><span class="keyword">, </span><span class="default">$level</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>now outputs:
<br>
<br>&quot;Notice: var must be numeric in doFunction called from main.php on line 4&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.trigger-error.php)

**[To root](/README.md)**