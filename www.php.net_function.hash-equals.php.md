# hash_equals




<div class="phpcode"><span class="html">
To transparently support this function on older versions of PHP use this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if(!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;hash_equals&apos;</span><span class="keyword">)) {<br>&#xA0; function </span><span class="default">hash_equals</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">, </span><span class="default">$str2</span><span class="keyword">) {<br>&#xA0; &#xA0; if(</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str1</span><span class="keyword">) != </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$str2</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$res </span><span class="keyword">= </span><span class="default">$str1 </span><span class="keyword">^ </span><span class="default">$str2</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$ret </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; for(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">--) </span><span class="default">$ret </span><span class="keyword">|= </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; return !</span><span class="default">$ret</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I don&apos;t know why asphp at dsgml dot com got that many downvotes, the function seems to work.<br><br>I extended it a bit to support strings of diffent length and to handle errors and ran some tests:<br><br>The test results and how to reproduce them: <a href="http://pastebin.com/mLMXJeva" rel="nofollow" target="_blank">http://pastebin.com/mLMXJeva</a><br><br>The function:<br><span class="default">&lt;?php<br><br></span><span class="keyword">if (!</span><span class="default">function_exists</span><span class="keyword">(</span><span class="string">&apos;hash_equals&apos;</span><span class="keyword">)) {<br><br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Timing attack safe string comparison<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * Compares two strings using the same time whether they&apos;re equal or not.<br>&#xA0; &#xA0;&#xA0; * This function should be used to mitigate timing attacks; for instance, when testing crypt() password hashes.<br>&#xA0; &#xA0;&#xA0; * <br>&#xA0; &#xA0;&#xA0; * @param string $known_string The string of known length to compare against<br>&#xA0; &#xA0;&#xA0; * @param string $user_string The user-supplied string<br>&#xA0; &#xA0;&#xA0; * @return boolean Returns TRUE when the two strings are equal, FALSE otherwise.<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">hash_equals</span><span class="keyword">(</span><span class="default">$known_string</span><span class="keyword">, </span><span class="default">$user_string</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">func_num_args</span><span class="keyword">() !== </span><span class="default">2</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// handle wrong parameter count as the native implentation<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="string">&apos;hash_equals() expects exactly 2 parameters, &apos; </span><span class="keyword">. </span><span class="default">func_num_args</span><span class="keyword">() . </span><span class="string">&apos; given&apos;</span><span class="keyword">, </span><span class="default">E_USER_WARNING</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$known_string</span><span class="keyword">) !== </span><span class="default">true</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="string">&apos;hash_equals(): Expected known_string to be a string, &apos; </span><span class="keyword">. </span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$known_string</span><span class="keyword">) . </span><span class="string">&apos; given&apos;</span><span class="keyword">, </span><span class="default">E_USER_WARNING</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$known_string_len </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$known_string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_string_type_error </span><span class="keyword">= </span><span class="string">&apos;hash_equals(): Expected user_string to be a string, &apos; </span><span class="keyword">. </span><span class="default">gettype</span><span class="keyword">(</span><span class="default">$user_string</span><span class="keyword">) . </span><span class="string">&apos; given&apos;</span><span class="keyword">; </span><span class="comment">// prepare wrong type error message now to reduce the impact of string concatenation and the gettype call<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$user_string</span><span class="keyword">) !== </span><span class="default">true</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">trigger_error</span><span class="keyword">(</span><span class="default">$user_string_type_error</span><span class="keyword">, </span><span class="default">E_USER_WARNING</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// prevention of timing attacks might be still possible if we handle $user_string as a string of diffent length (the trigger_error() call increases the execution time a bit)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_string_len </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$user_string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_string_len </span><span class="keyword">= </span><span class="default">$known_string_len </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_string_len </span><span class="keyword">= </span><span class="default">$known_string_len </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_string_len </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$user_string</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$known_string_len </span><span class="keyword">!== </span><span class="default">$user_string_len</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$res </span><span class="keyword">= </span><span class="default">$known_string </span><span class="keyword">^ </span><span class="default">$known_string</span><span class="keyword">; </span><span class="comment">// use $known_string instead of $user_string to handle strings of diffrent length.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ret </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="comment">// set $ret to 1 to make sure false is returned<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">} else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$res </span><span class="keyword">= </span><span class="default">$known_string </span><span class="keyword">^ </span><span class="default">$user_string</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ret </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">--) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ret </span><span class="keyword">|= </span><span class="default">ord</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$ret </span><span class="keyword">=== </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-equals.php)

**[To root](/README.md)**