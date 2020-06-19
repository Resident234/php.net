# nl2br




<div class="phpcode"><span class="html">
It&apos;s important to remember that this function does NOT replace newlines with &lt;br&gt; tags. Rather, it inserts a &lt;br&gt; tag before each newline, but it still preserves the newlines themselves! This caused problems for me regarding a function I was writing -- I forgot the newlines were still being preserved.
<br>
<br>If you don&apos;t want newlines, do:
<br>
<br><span class="default">&lt;?php
<br>$Result </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">( </span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">, </span><span class="default">$Text </span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
to replace all linebreaks to &lt;br /&gt;
<br>the best solution (IMO) is:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">nl2br2</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">) {
<br></span><span class="default">$string </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\r&quot;</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">), </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);
<br>return </span><span class="default">$string</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>because each OS have different ASCII chars for linebreak:
<br>windows = \r\n
<br>unix = \n
<br>mac = \r
<br>
<br>works perfect for me</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a more simple one:<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Convert BR tags to nl<br> *<br> * @param string The string to convert<br> * @return string The converted string<br> */<br></span><span class="keyword">function </span><span class="default">br2nl</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\&lt;br(\s*)?\/?\&gt;/i&apos;</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>Enjoy</span>
</div>
  

#


<div class="phpcode"><span class="html">
Starting from PHP 4.3.10 and PHP 5.0.2, this should be the most correct way to replace &lt;br /&gt; and &lt;br&gt; tags with newlines and carriage returns.<br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Convert BR tags to newlines and carriage returns.<br> *<br> * @param string The string to convert<br> * @return string The converted string<br> */<br></span><span class="keyword">function </span><span class="default">br2nl </span><span class="keyword">( </span><span class="default">$string </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\&lt;br(\s*)?\/?\&gt;/i&apos;</span><span class="keyword">, </span><span class="default">PHP_EOL</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span>(Please note this is a minor edit of this function: <a href="http://php.net/nl2br#86678" rel="nofollow" target="_blank">http://php.net/nl2br#86678</a> )<br><br>You might also want to be &quot;platform specific&quot;, and therefore this function might be of some help:<br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Convert BR tags to newlines and carriage returns.<br> *<br> * @param string The string to convert<br> * @param string The string to use as line separator<br> * @return string The converted string<br> */<br></span><span class="keyword">function </span><span class="default">br2nl </span><span class="keyword">( </span><span class="default">$string</span><span class="keyword">, </span><span class="default">$separator </span><span class="keyword">= </span><span class="default">PHP_EOL </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$separator </span><span class="keyword">= </span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$separator</span><span class="keyword">, array(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\r&quot;</span><span class="keyword">, </span><span class="string">&quot;\r\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\n\r&quot;</span><span class="keyword">, </span><span class="default">chr</span><span class="keyword">(</span><span class="default">30</span><span class="keyword">), </span><span class="default">chr</span><span class="keyword">(</span><span class="default">155</span><span class="keyword">), </span><span class="default">PHP_EOL</span><span class="keyword">)) ? </span><span class="default">$separator </span><span class="keyword">: </span><span class="default">PHP_EOL</span><span class="keyword">;&#xA0; </span><span class="comment">// Checks if provided $separator is valid.<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\&lt;br(\s*)?\/?\&gt;/i&apos;</span><span class="keyword">, </span><span class="default">$separator</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Seeing all these suggestions on a br2nl function, I can also see that neither would work with a sloppy written html line break.. Users can&apos;t be trusted to write good code, we know that, and mixing case isn&apos;t too uncommon.<br><br>I think this little snippet would do most tricks, both XHTML style and HTML, even mixed case like &lt;Br&gt; &lt;bR /&gt; and even &lt;br&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &gt; or &lt;br&#xA0; &#xA0;&#xA0; /&gt;.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">br2nl</span><span class="keyword">(</span><span class="default">$text</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return&#xA0; </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/&lt;br\\s*?\/??&gt;/i&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$text</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.nl2br.php)

**[To root](/README.md)**