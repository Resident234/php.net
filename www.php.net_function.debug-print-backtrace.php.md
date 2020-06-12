# debug_print_backtrace




<div class="phpcode"><span class="html">
Another way to manipulate and print a backtrace, without using output buffering:<br><br><span class="default">&lt;?php<br></span><span class="comment">// print backtrace, getting rid of repeated absolute path on each file<br></span><span class="default">$e </span><span class="keyword">= new </span><span class="default">Exception</span><span class="keyword">();<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&apos;/path/to/code/&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getTraceAsString</span><span class="keyword">()));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I like the output of debug_print_backtrace() but I sometimes want it as a string.
<br>
<br>bortuzar&apos;s solution to use output buffering is great, but I&apos;d like to factorize that into a function.&#xA0; Doing that however always results in whatever function name I use appearing at the top of the stack which is redundant.
<br>
<br>Below is my noddy (simple) solution.&#xA0; If you don&apos;t care for renumbering the call stack, omit the second preg_replace().
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">debug_string_backtrace</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ob_start</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">debug_print_backtrace</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$trace </span><span class="keyword">= </span><span class="default">ob_get_contents</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ob_end_clean</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Remove first item from backtrace as it&apos;s this function which
<br>&#xA0; &#xA0; &#xA0; &#xA0; // is redundant.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$trace </span><span class="keyword">= </span><span class="default">preg_replace </span><span class="keyword">(</span><span class="string">&apos;/^#0\s+&apos; </span><span class="keyword">. </span><span class="default">__FUNCTION__ </span><span class="keyword">. </span><span class="string">&quot;[^\n]*\n/&quot;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$trace</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Renumber backtrace items.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$trace </span><span class="keyword">= </span><span class="default">preg_replace </span><span class="keyword">(</span><span class="string">&apos;/^#(\d+)/me&apos;</span><span class="keyword">, </span><span class="string">&apos;\&apos;#\&apos; . ($1 - 1)&apos;</span><span class="keyword">, </span><span class="default">$trace</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$trace</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.debug-print-backtrace.php)

**[To root](/)**