# session_reset




<div class="phpcode"><span class="html">
First of all you should execute this code :<br><span class="default">&lt;?php<br>&#xA0; &#xA0; session_start</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&quot;A&quot;</span><span class="keyword">] = </span><span class="string">&quot;Some Value&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>then you should execute this one : <br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; start_session</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&quot;A&quot;</span><span class="keyword">] = </span><span class="string">&quot;Some New Value&quot;</span><span class="keyword">;&#xA0; </span><span class="comment">// set new value<br><br>&#xA0; &#xA0; </span><span class="default">session_reset</span><span class="keyword">();&#xA0; </span><span class="comment">// old session value restored<br>&#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&quot;A&quot;</span><span class="keyword">];<br><br>&#xA0; &#xA0; </span><span class="comment">//Output: Some Value<br></span><span class="default">?&gt;<br></span><br>That is because session_reset() is rolling back changes to the last saved session data, which is their values right after the session_start().</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-reset.php)

**[To root](/README.md)**