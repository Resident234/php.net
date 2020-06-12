# json_last_error




<div class="phpcode"><span class="html">
While this can obviously change between versions, the current error codes are as follows:<br><br>0 = JSON_ERROR_NONE<br>1 = JSON_ERROR_DEPTH<br>2 = JSON_ERROR_STATE_MISMATCH<br>3 = JSON_ERROR_CTRL_CHAR<br>4 = JSON_ERROR_SYNTAX<br>5 = JSON_ERROR_UTF8<br><br>I&apos;m only posting these for people who may be trying to understand why specific JSON files are not being decoded. Please do not hard-code these numbers into an error handler routine.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I used this simple script, flicked from StackOverflow to escape from the function failing:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">utf8ize</span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$d </span><span class="keyword">as </span><span class="default">$k </span><span class="keyword">=&gt; </span><span class="default">$v</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$d</span><span class="keyword">[</span><span class="default">$k</span><span class="keyword">] = </span><span class="default">utf8ize</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; } else if (</span><span class="default">is_string </span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">utf8_encode</span><span class="keyword">(</span><span class="default">$d</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$d</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Cheers,<br>Praveen Kumar!</span>
</div>
  

#


<div class="phpcode"><span class="html">
when json_decode a empty string, PHP7 will trigger an Syntax error:<br><span class="default">&lt;?php<br>json_decode</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_last_error</span><span class="keyword">(), </span><span class="default">json_last_error_msg</span><span class="keyword">());<br><br></span><span class="comment">// PHP 7<br></span><span class="default">int</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">)<br></span><span class="default">string</span><span class="keyword">(</span><span class="default">12</span><span class="keyword">) </span><span class="string">&quot;Syntax error&quot;<br><br></span><span class="comment">//&#xA0; PHP 5<br></span><span class="default">int</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">)<br></span><span class="default">string</span><span class="keyword">(</span><span class="default">8</span><span class="keyword">) </span><span class="string">&quot;No error&quot;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.json-last-error.php)

**[⬆ to root](/)**