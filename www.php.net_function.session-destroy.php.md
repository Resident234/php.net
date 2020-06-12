# session_destroy




<div class="phpcode"><span class="html">
If you want to change the session id on each log in, make sure to use session_regenerate_id(true) during the log in process.
<br>
<br><span class="default">&lt;?php
<br>session_start</span><span class="keyword">();
<br></span><span class="default">session_regenerate_id</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>[Edited by moderator (googleguy at php dot net)]</span>
</div>
  

#


<div class="phpcode"><span class="html">
It took me a while to figure out how to destroy a particular session in php. Note I&apos;m not sure if solution provided below is perfect but it seems work for me. Please feel free to post any easier way to destroy a particular session. Because it&apos;s quite useful for functionality of force an user offline.<br><br>1. If you&apos;re using db or memcached to manage session, you can always delete that session entry directly from db or memcached.<br><br>2. Using generic php session methods to delete a particular session(by session id).<br><br><span class="default">&lt;?php<br>$session_id_to_destroy </span><span class="keyword">= </span><span class="string">&apos;nill2if998vhplq9f3pj08vjb1&apos;</span><span class="keyword">;<br></span><span class="comment">// 1. commit session if it&apos;s started.<br></span><span class="keyword">if (</span><span class="default">session_id</span><span class="keyword">()) {<br>&#xA0; &#xA0; </span><span class="default">session_commit</span><span class="keyword">();<br>}<br><br></span><span class="comment">// 2. store current session id<br></span><span class="default">session_start</span><span class="keyword">();<br></span><span class="default">$current_session_id </span><span class="keyword">= </span><span class="default">session_id</span><span class="keyword">();<br></span><span class="default">session_commit</span><span class="keyword">();<br><br></span><span class="comment">// 3. hijack then destroy session specified.<br></span><span class="default">session_id</span><span class="keyword">(</span><span class="default">$session_id_to_destroy</span><span class="keyword">);<br></span><span class="default">session_start</span><span class="keyword">();<br></span><span class="default">session_destroy</span><span class="keyword">();<br></span><span class="default">session_commit</span><span class="keyword">();<br><br></span><span class="comment">// 4. restore current session id. If don&apos;t restore it, your current session will refer to the session you just destroyed!<br></span><span class="default">session_id</span><span class="keyword">(</span><span class="default">$current_session_id</span><span class="keyword">);<br></span><span class="default">session_start</span><span class="keyword">();<br></span><span class="default">session_commit</span><span class="keyword">();<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-destroy.php)

**[⬆ to root](/)**