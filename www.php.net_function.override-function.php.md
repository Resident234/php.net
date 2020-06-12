# override_function




<div class="phpcode"><span class="html">
I thought the example was not very helpful, because it doesn&apos;t even override the function with another function.<br>My question was: If I override a function, can I call the ORIGINAL function within the OVERRIDING function?<br>ie, can I do this:<br><span class="default">&lt;?php<br>override_function</span><span class="keyword">(</span><span class="string">&apos;strlen&apos;</span><span class="keyword">, </span><span class="string">&apos;$string&apos;</span><span class="keyword">, </span><span class="string">&apos;return override_strlen($string);&apos;</span><span class="keyword">);<br>function </span><span class="default">override_strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);&#xA0; <br>}<br></span><span class="default">?&gt;<br></span>The answer: NO, you will get a segfault.<br><br>HOWEVER, if you use rename_function to rename the original function to a third name, then call the third name in the OVERRIDING function, you will get the desired effect:<br><span class="default">&lt;?php<br>rename_function</span><span class="keyword">(</span><span class="string">&apos;strlen&apos;</span><span class="keyword">, </span><span class="string">&apos;new_strlen&apos;</span><span class="keyword">);<br></span><span class="default">override_function</span><span class="keyword">(</span><span class="string">&apos;strlen&apos;</span><span class="keyword">, </span><span class="string">&apos;$string&apos;</span><span class="keyword">, </span><span class="string">&apos;return override_strlen($string);&apos;</span><span class="keyword">);<br><br>function </span><span class="default">override_strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">new_strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">);&#xA0; <br>}<br></span><span class="default">?&gt;<br></span><br>I plan to use this functionality to generate log reports every time a function is called, with the parameters, time, result, etc... So to wrap a function in logging, that was what I had to do.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Overriden function name becomes __overridden__(). That&apos;s why you can&apos;t override two function, and that&apos;s how you can use the original function in the override.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.override-function.php)

**[To root](/)**