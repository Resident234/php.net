# session_set_save_handler




<div class="phpcode"><span class="html">
As of PHP 7.0, you can implement SessionUpdateTimestampHandlerInterface to <br>define your own session id validating method like validate_sid and the timestamp updating method like update_timestamp in the non-OOP prototype of session_set_save_handler().<br><br>SessionUpdateTimestampHandlerInterface is a new interface introduced in PHP 7.0, which has not been documented yet. It has two abstract methods: SessionUpdateTimestampHandlerInterface :: validateId($sessionId) and SessionUpdateTimestampHandlerInterface :: updateTimestamp($sessionId, $sessionData).<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">/*<br>&#xA0; &#xA0; &#xA0;&#xA0; @author Wu Xiancheng<br>&#xA0; &#xA0; &#xA0;&#xA0; Code structure for PHP 7.0+ only because SessionUpdateTimestampHandlerInterface is introduced in PHP 7.0<br>&#xA0; &#xA0; &#xA0;&#xA0; With this class you can validate php session id and update the timestamp of php session data<br>&#xA0; &#xA0; &#xA0;&#xA0; with the OOP prototype of session_set_save_handler() in PHP 7.0+<br>&#xA0; &#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">class </span><span class="default">PHPSessionXHandler </span><span class="keyword">implements </span><span class="default">SessionHandlerInterface</span><span class="keyword">, </span><span class="default">SessionUpdateTimestampHandlerInterface </span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">close</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// return value should be true for success or false for failure<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">destroy</span><span class="keyword">(</span><span class="default">$sessionId</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// return value should be true for success or false for failure<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ... <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">gc</span><span class="keyword">(</span><span class="default">$maximumLifetime</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// return value should be true for success or false for failure<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">open</span><span class="keyword">(</span><span class="default">$sessionSavePath</span><span class="keyword">, </span><span class="default">$sessionName</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// return value should be true for success or false for failure<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">read</span><span class="keyword">(</span><span class="default">$sessionId</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// return value should be the session data or an empty string<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">write</span><span class="keyword">(</span><span class="default">$sessionId</span><span class="keyword">, </span><span class="default">$sessionData</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// return value should be true for success or false for failure<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">create_sid</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// available since PHP 5.5.1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // invoked internally when a new session id is needed<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // no parameter is needed and return value should be the new session id created<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">validateId</span><span class="keyword">(</span><span class="default">$sessionId</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// implements SessionUpdateTimestampHandlerInterface::validateId()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // available since PHP 7.0<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // return value should be true if the session id is valid otherwise false<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // if false is returned a new session id will be generated by php internally<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">updateTimestamp</span><span class="keyword">(</span><span class="default">$sessionId</span><span class="keyword">, </span><span class="default">$sessionData</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// implements SessionUpdateTimestampHandlerInterface::validateId()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // available since PHP 7.0<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // return value should be true for success or false for failure<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ...<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
After spend so many time to understand how PHP session works with database and unsuccessful attempts to get it right, I decided to rewrite the version from our friend stalker.<br><br>//Database<br>CREATE TABLE `Session` (<br>&#xA0; `Session_Id` varchar(255) COLLATE utf8_unicode_ci NOT NULL,<br>&#xA0; `Session_Expires` datetime NOT NULL,<br>&#xA0; `Session_Data` text COLLATE utf8_unicode_ci,<br>&#xA0; PRIMARY KEY (`Session_Id`)<br>) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;<br>SELECT * FROM mydatabase.Session;<br><br><span class="default">&lt;?php<br></span><span class="comment">//inc.session.php<br><br></span><span class="keyword">class </span><span class="default">SysSession </span><span class="keyword">implements </span><span class="default">SessionHandlerInterface<br></span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$link</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">open</span><span class="keyword">(</span><span class="default">$savePath</span><span class="keyword">, </span><span class="default">$sessionName</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$link </span><span class="keyword">= </span><span class="default">mysqli_connect</span><span class="keyword">(</span><span class="string">&quot;server&quot;</span><span class="keyword">,</span><span class="string">&quot;user&quot;</span><span class="keyword">,</span><span class="string">&quot;pwd&quot;</span><span class="keyword">,</span><span class="string">&quot;mydatabase&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$link</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">link </span><span class="keyword">= </span><span class="default">$link</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">close</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mysqli_close</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">link</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">read</span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">link</span><span class="keyword">,</span><span class="string">&quot;SELECT Session_Data FROM Session WHERE Session_Id = &apos;&quot;</span><span class="keyword">.</span><span class="default">$id</span><span class="keyword">.</span><span class="string">&quot;&apos; AND Session_Expires &gt; &apos;&quot;</span><span class="keyword">.</span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">).</span><span class="string">&quot;&apos;&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysqli_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;Session_Data&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&quot;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">write</span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$DateTime </span><span class="keyword">= </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$NewDateTime </span><span class="keyword">= </span><span class="default">date</span><span class="keyword">(</span><span class="string">&apos;Y-m-d H:i:s&apos;</span><span class="keyword">,</span><span class="default">strtotime</span><span class="keyword">(</span><span class="default">$DateTime</span><span class="keyword">.</span><span class="string">&apos; + 1 hour&apos;</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">link</span><span class="keyword">,</span><span class="string">&quot;REPLACE INTO Session SET Session_Id = &apos;&quot;</span><span class="keyword">.</span><span class="default">$id</span><span class="keyword">.</span><span class="string">&quot;&apos;, Session_Expires = &apos;&quot;</span><span class="keyword">.</span><span class="default">$NewDateTime</span><span class="keyword">.</span><span class="string">&quot;&apos;, Session_Data = &apos;&quot;</span><span class="keyword">.</span><span class="default">$data</span><span class="keyword">.</span><span class="string">&quot;&apos;&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$result</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">destroy</span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">link</span><span class="keyword">,</span><span class="string">&quot;DELETE FROM Session WHERE Session_Id =&apos;&quot;</span><span class="keyword">.</span><span class="default">$id</span><span class="keyword">.</span><span class="string">&quot;&apos;&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$result</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; public function </span><span class="default">gc</span><span class="keyword">(</span><span class="default">$maxlifetime</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">link</span><span class="keyword">,</span><span class="string">&quot;DELETE FROM Session WHERE ((UNIX_TIMESTAMP(Session_Expires) + &quot;</span><span class="keyword">.</span><span class="default">$maxlifetime</span><span class="keyword">.</span><span class="string">&quot;) &lt; &quot;</span><span class="keyword">.</span><span class="default">$maxlifetime</span><span class="keyword">.</span><span class="string">&quot;)&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$result</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">$handler </span><span class="keyword">= new </span><span class="default">SysSession</span><span class="keyword">();<br></span><span class="default">session_set_save_handler</span><span class="keyword">(</span><span class="default">$handler</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="comment">//page 1<br></span><span class="keyword">require_once(</span><span class="string">&apos;inc.session.php&apos;</span><span class="keyword">);<br><br></span><span class="default">session_start</span><span class="keyword">();<br><br></span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;var1&apos;</span><span class="keyword">] = </span><span class="string">&quot;My Portuguese text: SOU Gaucho!&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br></span><span class="comment">//page 2<br></span><span class="keyword">require_once(</span><span class="string">&apos;inc.session.php&apos;</span><span class="keyword">);<br><br></span><span class="default">session_start</span><span class="keyword">();<br><br>if(isset(</span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;var1&apos;</span><span class="keyword">]){<br>echo </span><span class="default">$_SESSION</span><span class="keyword">[</span><span class="string">&apos;var1&apos;</span><span class="keyword">]; <br>}<br></span><span class="comment">//OUTPUT: My Portuguese text: SOU Gaucho!<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-set-save-handler.php)

**[To root](/README.md)**