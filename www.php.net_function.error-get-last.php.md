# error_get_last




<div class="phpcode"><span class="html">
[Editor&apos;s note: as of PHP 7.0.0 there is error_clear_last() to clear the most recent error.]
<br>
<br>To clear error_get_last(), or put it in a well defined state, you should use the code below. It works even when a custom error handler has been set.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">// var_dump or anything else, as this will never be called because of the 0
<br></span><span class="default">set_error_handler</span><span class="keyword">(</span><span class="string">&apos;var_dump&apos;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);
<br>@</span><span class="default">$undef_var</span><span class="keyword">;
<br></span><span class="default">restore_error_handler</span><span class="keyword">();
<br>
<br></span><span class="comment">// error_get_last() is now in a well known state:
<br>// Undefined variable: undef_var
<br>
<br></span><span class="keyword">... </span><span class="comment">// Do something
<br>
<br></span><span class="default">$e </span><span class="keyword">= </span><span class="default">error_get_last</span><span class="keyword">();
<br>
<br>...
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If an error handler (see set_error_handler ) successfully handles an error then that error will not be reported by this function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The error_get_last() function will give you the most recent error even when that error is a Fatal error.<br><br>Example Usage:<br><br><span class="default">&lt;?php<br><br>register_shutdown_function</span><span class="keyword">(</span><span class="string">&apos;handleFatalPhpError&apos;</span><span class="keyword">);<br><br>function </span><span class="default">handleFatalPhpError</span><span class="keyword">() {<br>&#xA0;&#xA0; </span><span class="default">$last_error </span><span class="keyword">= </span><span class="default">error_get_last</span><span class="keyword">();<br>&#xA0;&#xA0; if(</span><span class="default">$last_error</span><span class="keyword">[</span><span class="string">&apos;type&apos;</span><span class="keyword">] === </span><span class="default">E_ERROR</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Can do custom output and/or logging for fatal error here...&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.error-get-last.php)

**[⬆ to root](/)**