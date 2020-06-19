# session_unset




<div class="phpcode"><span class="html">
I was having a problem clearing all session variables, deleting the session, and creating a new session without leaving old session stuff behind in all browsers.&#xA0; The below code is perfect for a logout script to totally delete everything and start new.&#xA0; It even works in Chrome which seems to not work as other browsers when trying do logout and start a new session.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; session_start</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">session_unset</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">session_destroy</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">session_write_close</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">setcookie</span><span class="keyword">(</span><span class="default">session_name</span><span class="keyword">(),</span><span class="string">&apos;&apos;</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="string">&apos;/&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">session_regenerate_id</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The difference between both session_unset and session_destroy is as follows:<br><br>session_unset just clears out the session for usage. The session is still on the users computer. Note that by using session_unset, the variable still exists. session_unset just remove all session variables. it does not destroy the session....so the session would still be active.<br><br>Using session_unset in tandem with session_destroy however, is a much more effective means of actually clearing out data. As stated in the example above, this works very well, cross browser. session_destroy is destroy the session. session_destroy() to kill all session information.....This is the more secure function to use.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-unset.php)

**[To root](/README.md)**