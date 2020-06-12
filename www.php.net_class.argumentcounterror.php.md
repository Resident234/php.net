# ArgumentCountError




<div class="phpcode"><span class="html">
Note if an invalid number of arguments are passed to a built-in function an ArgumentCountError exception will be thrown if and only if your code is in strict mode.<br><br><span class="default">&lt;?php<br></span><span class="keyword">declare(</span><span class="default">strict_types </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">);<br><br>try {<br>&#xA0; &#xA0; echo </span><span class="default">strlen</span><span class="keyword">(</span><span class="string">&apos;ahmed&apos;</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">);<br>} catch (</span><span class="default">ArgumentCountError $e</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">()</span><span class="string">&apos;;<br>}<br>?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.argumentcounterror.php)

**[To root](/README.md)**