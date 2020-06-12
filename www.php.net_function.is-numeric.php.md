# is_numeric




<div class="phpcode"><span class="html">
If you want the numerical value of a string, this will return a float or int value:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">get_numeric</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) {
<br>&#xA0; if (</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">)) {
<br>&#xA0; &#xA0; return </span><span class="default">$val </span><span class="keyword">+ </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; return </span><span class="default">0</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Example:
<br><span class="default">&lt;?php
<br>get_numeric</span><span class="keyword">(</span><span class="string">&apos;3&apos;</span><span class="keyword">); </span><span class="comment">// int(3)
<br></span><span class="default">get_numeric</span><span class="keyword">(</span><span class="string">&apos;1.2&apos;</span><span class="keyword">); </span><span class="comment">// float(1.2)
<br></span><span class="default">get_numeric</span><span class="keyword">(</span><span class="string">&apos;3.0&apos;</span><span class="keyword">); </span><span class="comment">// float(3)
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that the function accepts extremely big numbers and correctly evaluates them.<br><br>For example:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $v </span><span class="keyword">= </span><span class="default">is_numeric </span><span class="keyword">(</span><span class="string">&apos;58635272821786587286382824657568871098287278276543219876543&apos;</span><span class="keyword">) ? </span><span class="default">true </span><span class="keyword">: </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">var_dump </span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The above script will output:<br><br>bool(true)<br><br>So this function is not intimidated by super-big numbers. I hope this helps someone.<br><br>PS: Also note that if you write is_numeric (45thg), this will generate a parse error (since the parameter is not enclosed between apostrophes or double quotes). Keep this in mind when you use this function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The documentation does not clarify what happens if you the input is an empty string - it correctly returns false in my experience.&#xA0; Useful to state these odd cases, for when you see code that checks for an empty string and is_numeric, you can tell it&apos;s a waste of a comparison.</span>
</div>
  

#


<div class="phpcode"><span class="html">
for strings, it return true only if float number has a dot<br><br>is_numeric( &apos;42.1&apos; )//true<br>is_numeric( &apos;42,1&apos; )//false</span>
</div>
  

#


<div class="phpcode"><span class="html">
The documentation is not completely precise here. is_numeric will also return true if the number begins with a decimal point&#xA0; and/or a space, provided a number follows (rather than a letter or punctuation). So, it doesn&apos;t necessarily have to start with a digit.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-numeric.php)

**[⬆ to root](/)**