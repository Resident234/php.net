# session_abort




<div class="phpcode"><span class="html">
To better understand this function you should execute this code first :<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">// First of all choose your path , For e.g. C:/session<br>&#xA0; &#xA0; </span><span class="default">session_save_path</span><span class="keyword">(</span><span class="string">&apos;Your Path here !&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">session_start</span><span class="keyword">();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// Define a Session Variable<br>&#xA0; &#xA0; </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;Key&apos;</span><span class="keyword">] = </span><span class="string">&apos;value&apos; </span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">Var_dump</span><span class="keyword">(</span><span class="default">session_status</span><span class="keyword">() == </span><span class="default">PHP_SESSION_ACTIVE</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// Output : bool(True) , it means you have an open session !<br></span><span class="default">?&gt;<br></span><br>Then you should execute this code :<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">// Choose the path that you used it in first part&#xA0; <br>&#xA0; &#xA0; </span><span class="default">session_save_path</span><span class="keyword">(</span><span class="string">&apos;Your path here&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">session_start</span><span class="keyword">();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// If you want to close session and keep your original data in your path , you should use session_abort()<br>&#xA0; &#xA0; </span><span class="default">session_abort</span><span class="keyword">();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">session_status</span><span class="keyword">()== </span><span class="default">PHP_SESSION_ACTIVE</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// Output : bool(False) , it means your session closed .<br></span><span class="default">?&gt;<br></span><br>So if you have an open session , session_abort() will simply close it without effecting the external session data , so you can reload your data again from your path that you chose .</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-abort.php)

**[⬆ to root](/)**