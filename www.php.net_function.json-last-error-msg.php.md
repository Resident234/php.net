# json_last_error_msg




<div class="phpcode"><span class="html">
Here&apos;s an updated version of the function:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;json_last_error_msg&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; function </span><span class="default">json_last_error_msg</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; static </span><span class="default">$ERRORS </span><span class="keyword">= array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">JSON_ERROR_NONE </span><span class="keyword">=&gt; </span><span class="string">&apos;No error&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">JSON_ERROR_DEPTH </span><span class="keyword">=&gt; </span><span class="string">&apos;Maximum stack depth exceeded&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">JSON_ERROR_STATE_MISMATCH </span><span class="keyword">=&gt; </span><span class="string">&apos;State mismatch (invalid or malformed JSON)&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">JSON_ERROR_CTRL_CHAR </span><span class="keyword">=&gt; </span><span class="string">&apos;Control character error, possibly incorrectly encoded&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">JSON_ERROR_SYNTAX </span><span class="keyword">=&gt; </span><span class="string">&apos;Syntax error&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">JSON_ERROR_UTF8 </span><span class="keyword">=&gt; </span><span class="string">&apos;Malformed UTF-8 characters, possibly incorrectly encoded&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$error </span><span class="keyword">= </span><span class="default">json_last_error</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return isset(</span><span class="default">$ERRORS</span><span class="keyword">[</span><span class="default">$error</span><span class="keyword">]) ? </span><span class="default">$ERRORS</span><span class="keyword">[</span><span class="default">$error</span><span class="keyword">] : </span><span class="string">&apos;Unknown error&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.json-last-error-msg.php)

**[⬆ to root](/)**