# ob_end_flush




<div class="phpcode"><span class="html">
A note on the above example...<br><br>with PHP 4 &gt;= 4.2.0, PHP 5 you can use a combination of ob_get_level() and ob_end_flush() to avoid using the @ (error suppresion) which should probably be a little faaster.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">while (</span><span class="default">ob_get_level</span><span class="keyword">() &gt; </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">ob_end_flush</span><span class="keyword">();<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
best way to compress a css code:<br><br><span class="default">&lt;?php<br>&#xA0; header</span><span class="keyword">(</span><span class="string">&apos;Content-type: text/css&apos;</span><span class="keyword">);<br><br>&#xA0; </span><span class="default">ob_start</span><span class="keyword">(</span><span class="string">&quot;compress&quot;</span><span class="keyword">);<br>&#xA0; function </span><span class="default">compress</span><span class="keyword">(</span><span class="default">$buffer</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// remove comments<br>&#xA0; &#xA0; </span><span class="default">$buffer </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;!/\*[^*]*\*+([^/][^*]*\*+)*/!&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$buffer</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="comment">// remove tabs, spaces, newlines, etc.<br>&#xA0; &#xA0; </span><span class="default">$buffer </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\r&quot;</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\t&quot;</span><span class="keyword">, </span><span class="string">&apos;&#xA0; &apos;</span><span class="keyword">, </span><span class="string">&apos;&#xA0; &#xA0; &apos;</span><span class="keyword">, </span><span class="string">&apos;&#xA0; &#xA0; &apos;</span><span class="keyword">), </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$buffer</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$buffer</span><span class="keyword">;<br>&#xA0; }<br><br>&#xA0; include(</span><span class="string">&apos;./template/main.css&apos;</span><span class="keyword">);<br>&#xA0; include(</span><span class="string">&apos;./template/classes.css&apos;</span><span class="keyword">);<br><br>&lt;?</span><span class="default">php<br>&#xA0; ob_end_flush</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>Include in &lt;head&gt;:<br>&lt;link rel=&quot;stylesheet&quot; type=&quot;text/css&quot; href=&quot;/design.php&quot; media=&quot;all&quot; /&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-end-flush.php)

**[⬆ to root](/)**