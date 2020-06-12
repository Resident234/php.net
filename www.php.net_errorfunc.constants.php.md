# Predefined Constants




<div class="phpcode"><span class="html">
[Editor&apos;s note: fixed E_COMPILE_* cases that incorrectly returned E_CORE_* strings. Thanks josiebgoode.]
<br>
<br>The following code expands on Vlad&apos;s code to show all the flags that are set.&#xA0; if not set, a blank line shows.
<br>
<br><span class="default">&lt;?php
<br>$errLvl </span><span class="keyword">= </span><span class="default">error_reporting</span><span class="keyword">();
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">15</span><span class="keyword">;&#xA0; </span><span class="default">$i</span><span class="keyword">++ ) {
<br>&#xA0; &#xA0; print </span><span class="default">FriendlyErrorType</span><span class="keyword">(</span><span class="default">$errLvl </span><span class="keyword">&amp; </span><span class="default">pow</span><span class="keyword">(</span><span class="default">2</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">)) . </span><span class="string">&quot;&lt;br&gt;\\n&quot;</span><span class="keyword">; 
<br>}
<br>
<br>function </span><span class="default">FriendlyErrorType</span><span class="keyword">(</span><span class="default">$type</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; switch(</span><span class="default">$type</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_ERROR</span><span class="keyword">: </span><span class="comment">// 1 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_ERROR&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_WARNING</span><span class="keyword">: </span><span class="comment">// 2 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_WARNING&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_PARSE</span><span class="keyword">: </span><span class="comment">// 4 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_PARSE&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_NOTICE</span><span class="keyword">: </span><span class="comment">// 8 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_NOTICE&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_CORE_ERROR</span><span class="keyword">: </span><span class="comment">// 16 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_CORE_ERROR&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_CORE_WARNING</span><span class="keyword">: </span><span class="comment">// 32 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_CORE_WARNING&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_COMPILE_ERROR</span><span class="keyword">: </span><span class="comment">// 64 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_COMPILE_ERROR&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_COMPILE_WARNING</span><span class="keyword">: </span><span class="comment">// 128 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_COMPILE_WARNING&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_ERROR</span><span class="keyword">: </span><span class="comment">// 256 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_USER_ERROR&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_WARNING</span><span class="keyword">: </span><span class="comment">// 512 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_USER_WARNING&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_NOTICE</span><span class="keyword">: </span><span class="comment">// 1024 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_USER_NOTICE&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_STRICT</span><span class="keyword">: </span><span class="comment">// 2048 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_STRICT&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_RECOVERABLE_ERROR</span><span class="keyword">: </span><span class="comment">// 4096 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_RECOVERABLE_ERROR&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_DEPRECATED</span><span class="keyword">: </span><span class="comment">// 8192 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_DEPRECATED&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">E_USER_DEPRECATED</span><span class="keyword">: </span><span class="comment">// 16384 //
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="string">&apos;E_USER_DEPRECATED&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
-1 is also semantically meaningless as a bit field, and only works in 2s-complement numeric representations.&#xA0; On a 1s-complement system -1 would not set E_ERROR.&#xA0; On a sign-magnitude system -1 would set nothing at all! (see e.g. <a href="http://en.wikipedia.org/wiki/Ones%27_complement" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Ones%27_complement</a>)<br><br>If you want to set all bits, ~0 is the correct way to do it.<br><br>But setting undefined bits could result in undefined behaviour and that means *absolutely anything* could happen :-)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/errorfunc.constants.php)

**[To root](/)**