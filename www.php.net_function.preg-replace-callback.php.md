# preg_replace_callback




<div class="phpcode"><span class="html">
The easiest way to pass more than one parameters to the callback function is with the &apos;use&apos; keyword. 
<br>
<br>[This is better than using global, because it works even when we are already inside a function.]
<br>
<br>In this example, the callback function is an anonymous function, which takes one argument, $match, supplied by preg_replace_callback().&#xA0; The extra 
<br>&quot;use ($ten)&quot; puts the $ten variable into scope for the function.
<br>
<br><span class="default">&lt;?php
<br>$string </span><span class="keyword">= </span><span class="string">&quot;Some numbers: one: 1; two: 2; three: 3 end&quot;</span><span class="keyword">;
<br></span><span class="default">$ten </span><span class="keyword">= </span><span class="default">10</span><span class="keyword">;
<br></span><span class="default">$newstring </span><span class="keyword">= </span><span class="default">preg_replace_callback</span><span class="keyword">(
<br>&#xA0; &#xA0; </span><span class="string">&apos;/(\\d+)/&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; function(</span><span class="default">$match</span><span class="keyword">) use (</span><span class="default">$ten</span><span class="keyword">) { return ((</span><span class="default">$match</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] + </span><span class="default">$ten</span><span class="keyword">)); },
<br>&#xA0; &#xA0; </span><span class="default">$string
<br>&#xA0; &#xA0; </span><span class="keyword">);
<br>echo </span><span class="default">$newstring</span><span class="keyword">;
<br></span><span class="comment">#prints &quot;Some numbers: one: 11; two: 12; three: 13 end&quot;;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to call non-static function inside your class, you can do something like this. <br><br>For PHP 5.2 use second argument like array($this, &apos;replace&apos;):<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">test_preg_callback</span><span class="keyword">{<br><br>&#xA0; private function </span><span class="default">process</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$reg </span><span class="keyword">= </span><span class="string">&quot;/\{([0-9a-zA-Z\- ]+)\:([0-9a-zA-Z\- ]+):?\}/&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="default">preg_replace_callback</span><span class="keyword">(</span><span class="default">$reg</span><span class="keyword">, array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;replace&apos;</span><span class="keyword">), </span><span class="default">$text</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; <br>&#xA0; private function </span><span class="default">replace</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">){<br>&#xA0; &#xA0; if (</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">])){<br>&#xA0; &#xA0; &#xA0; return @</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">](</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]);&#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; }<br>&#xA0; }&#xA0; <br>}<br></span><span class="default">?&gt;<br></span><br>For PHP 5.3 use second argument like &quot;self::replace&quot;:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">test_preg_callback</span><span class="keyword">{<br><br>&#xA0; private function </span><span class="default">process</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$reg </span><span class="keyword">= </span><span class="string">&quot;/\{([0-9a-zA-Z\- ]+)\:([0-9a-zA-Z\- ]+):?\}/&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="default">preg_replace_callback</span><span class="keyword">(</span><span class="default">$reg</span><span class="keyword">, </span><span class="string">&quot;self::replace&quot;</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; <br>&#xA0; private function </span><span class="default">replace</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">){<br>&#xA0; &#xA0; if (</span><span class="default">method_exists</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">])){<br>&#xA0; &#xA0; &#xA0; return @</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">](</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]);&#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; }<br>&#xA0; }&#xA0; <br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-replace-callback.php)

**[⬆ to root](/)**