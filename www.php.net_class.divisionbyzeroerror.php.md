# DivisionByZeroError




<div class="phpcode"><span class="html">
Note that on division by zero 1/0 and module by zero 1%0 an E_WARNING is triggered first (probably for backward compatibility with PHP5), then the DivisionByZeroError exception is thrown next.<br><br>The result is, for example, that if you set the maximum level of error detection with error_level(-1) and you also map errors to exception, say ErrorException, then on division by zero only this latter exception is thrown reporting &quot;Division by zero&quot;. The result is that a code like this:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Set a safe environment:<br></span><span class="default">error_reporting</span><span class="keyword">(-</span><span class="default">1</span><span class="keyword">);<br><br></span><span class="comment">// Maps errors to ErrorException.<br></span><span class="keyword">function </span><span class="default">my_error_handler</span><span class="keyword">(</span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$message</span><span class="keyword">)<br>{ throw new </span><span class="default">ErrorException</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">); }<br><br>try {<br>&#xA0; &#xA0; echo </span><span class="default">1</span><span class="keyword">/</span><span class="default">0</span><span class="keyword">;<br>}<br>catch(</span><span class="default">ErrorException $e</span><span class="keyword">){<br>&#xA0; &#xA0; echo </span><span class="string">&quot;got </span><span class="default">$e</span><span class="string">&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>allows to detect such error in the same way under PHP5 and PHP7, although the DivisionByZeroError exception is masked off by ErrorException.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.divisionbyzeroerror.php)

**[⬆ to root](/)**