# session_status




<div class="phpcode"><span class="html">
Maybe depending on PHP settings, but if return values are not the above, then go for this:<br>_DISABLED = 0<br>_NONE = 1<br>_ACTIVE = 2</span>
</div>
  

#


<div class="phpcode"><span class="html">
Universal function for checking session status.<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * @return bool<br> */<br></span><span class="keyword">function </span><span class="default">is_session_started</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; if ( </span><span class="default">php_sapi_name</span><span class="keyword">() !== </span><span class="string">&apos;cli&apos; </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( </span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&apos;5.4.0&apos;</span><span class="keyword">, </span><span class="string">&apos;&gt;=&apos;</span><span class="keyword">) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">session_status</span><span class="keyword">() === </span><span class="default">PHP_SESSION_ACTIVE </span><span class="keyword">? </span><span class="default">TRUE </span><span class="keyword">: </span><span class="default">FALSE</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">session_id</span><span class="keyword">() === </span><span class="string">&apos;&apos; </span><span class="keyword">? </span><span class="default">FALSE </span><span class="keyword">: </span><span class="default">TRUE</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">FALSE</span><span class="keyword">;<br>}<br><br></span><span class="comment">// Example<br></span><span class="keyword">if ( </span><span class="default">is_session_started</span><span class="keyword">() === </span><span class="default">FALSE </span><span class="keyword">) </span><span class="default">session_start</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-status.php)

**[To root](/README.md)**