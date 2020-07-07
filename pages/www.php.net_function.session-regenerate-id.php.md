# session_regenerate_id




<div class="phpcode"><span class="html">
I wrote the current top voted comment on this and wanted to add something. The existing code from my previous comment generates it&apos;s nonces in an insecure way-<br><br><span class="default">&lt;?php<br>$_SESSION</span><span class="keyword">[</span><span class="string">&apos;nonce&apos;</span><span class="keyword">] = </span><span class="default">md5</span><span class="keyword">(</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Since &quot;microtime&quot; is predictable it makes brute forcing the nonce much easier. A better option would be something that utilizes randomness, such as-<br><br><span class="default">&lt;?php<br>bin2hex</span><span class="keyword">(</span><span class="default">openssl_random_pseudo_bytes</span><span class="keyword">(</span><span class="default">32</span><span class="keyword">))<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I wrote the following code for a project I&apos;m working on- it attempts to resolve the regenerate issue, as well as deal with a couple of other session related things.<br><br>I tried to make it a little more generic and usable (for instance, in the full version it throws different types of exceptions for the different types of session issues), so hopefully someone might find it useful.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">regenerateSession</span><span class="keyword">(</span><span class="default">$reload </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// This token is used by forms to prevent cross site forgery attempts<br>&#xA0; &#xA0; </span><span class="keyword">if(!isset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;nonce&apos;</span><span class="keyword">]) || </span><span class="default">$reload</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;nonce&apos;</span><span class="keyword">] = </span><span class="default">md5</span><span class="keyword">(</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">));<br><br>&#xA0; &#xA0; if(!isset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;IPaddress&apos;</span><span class="keyword">]) || </span><span class="default">$reload</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;IPaddress&apos;</span><span class="keyword">] = </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REMOTE_ADDR&apos;</span><span class="keyword">];<br><br>&#xA0; &#xA0; if(!isset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;userAgent&apos;</span><span class="keyword">]) || </span><span class="default">$reload</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;userAgent&apos;</span><span class="keyword">] = </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">];<br><br>&#xA0; &#xA0; </span><span class="comment">//$_SESSION[&apos;user_id&apos;] = $this-&gt;user-&gt;getId();<br><br>&#xA0; &#xA0; // Set current session to expire in 1 minute<br>&#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;OBSOLETE&apos;</span><span class="keyword">] = </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;EXPIRES&apos;</span><span class="keyword">] = </span><span class="default">time</span><span class="keyword">() + </span><span class="default">60</span><span class="keyword">;<br><br>&#xA0; &#xA0; </span><span class="comment">// Create new session without destroying the old one<br>&#xA0; &#xA0; </span><span class="default">session_regenerate_id</span><span class="keyword">(</span><span class="default">false</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// Grab current session ID and close both sessions to allow other scripts to use them<br>&#xA0; &#xA0; </span><span class="default">$newSession </span><span class="keyword">= </span><span class="default">session_id</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">session_write_close</span><span class="keyword">();<br><br>&#xA0; &#xA0; </span><span class="comment">// Set session ID to the new one, and start it back up again<br>&#xA0; &#xA0; </span><span class="default">session_id</span><span class="keyword">(</span><span class="default">$newSession</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">session_start</span><span class="keyword">();<br><br>&#xA0; &#xA0; </span><span class="comment">// Don&apos;t want this one to expire<br>&#xA0; &#xA0; </span><span class="keyword">unset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;OBSOLETE&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; unset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;EXPIRES&apos;</span><span class="keyword">]);<br>}<br><br>function </span><span class="default">checkSession</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; try{<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;OBSOLETE&apos;</span><span class="keyword">] &amp;&amp; (</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;EXPIRES&apos;</span><span class="keyword">] &lt; </span><span class="default">time</span><span class="keyword">()))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Attempt to use expired session.&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;user_id&apos;</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;No session started.&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;IPaddress&apos;</span><span class="keyword">] != </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REMOTE_ADDR&apos;</span><span class="keyword">])<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;IP Address mixmatch (possible session hijacking attempt).&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;userAgent&apos;</span><span class="keyword">] != </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">])<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Useragent mixmatch (possible session hijacking attempt).&apos;</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">loadUser</span><span class="keyword">(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;user_id&apos;</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Attempted to log in user that does not exist with ID: &apos; </span><span class="keyword">. </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;user_id&apos;</span><span class="keyword">]);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;OBSOLETE&apos;</span><span class="keyword">] &amp;&amp; </span><span class="default">mt_rand</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">, </span><span class="default">100</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">regenerateSession</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br><br>&#xA0; &#xA0; }catch(</span><span class="default">Exception $e</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In PHP 5.6 (and probably older versions), session_regenerate_id(true) do not trigger a read() call to the session handler for the new session id. <br><br>In PHP 7, read() is triggered during session_regenerate_id(true). Nice to know when working with custom session handlers.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In a previous note, php at 5mm de describes how to prevent session hijacking by<br>ensuring that the session id provided matches the HTTP_USER_AGENT and REMOTE_ADDR fields that were present when the session id was first issued.&#xA0; It should be noted that HTTP_USER_AGENT is supplied by the client, and so can be easily modified by a malicious user.&#xA0; Also, the client IP addresses can be spoofed, although that&apos;s a bit more difficult.&#xA0; Care should be taken when relying on the session for authentication.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-regenerate-id.php)

**[To root](/README.md)**