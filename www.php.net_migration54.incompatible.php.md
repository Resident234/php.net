# Backward Incompatible Changes




<div class="phpcode"><span class="html">
If you have content that is not 100% UTF-8 then TAKE NOTE:<br><br>Starting in PHP 5.4 htmlspecialchars() and htmlentities() assume charset=UTF-8 by default AND WILL RETURN BLANK IF YOUR INPUT IS NOT VALID UTF-8.<br><br>So if you have a lot of function calls that look like this:<br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">htmlspecialchars</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">);<br></span><span class="comment">// or<br></span><span class="keyword">echo </span><span class="default">htmlentities</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>i.e. no charset and no flags -- and $input is ISO-8859 (or anything else apart from 7-bit ASCII or UTF-8) -- then PHP 5.4 and 5.5 will return an empty string, and you will be surprised and probably unhappy.<br><br>This is apparently a feature, not a bug.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Missing some chars like german umlauts after use of htmlspecialchars? That&apos;s because the third param encoding has changed it&apos;s default value in PHP 5.4 from ISO-8859-1 to UTF-8. 
<br>
<br>Possible solution #1:
<br>Change your code from this ...
<br><span class="default">&lt;?php htmlspecialchars</span><span class="keyword">( </span><span class="string">&apos;&#xE4;&#xF6;&#xFC;&apos; </span><span class="keyword">); </span><span class="default">?&gt;
<br></span>... to this:
<br><span class="default">&lt;?php htmlspecialchars </span><span class="keyword">( </span><span class="string">&apos;&#xE4;&#xF6;&#xFC;&apos; </span><span class="keyword">, </span><span class="default">ENT_COMPAT </span><span class="keyword">| </span><span class="default">ENT_HTML401 </span><span class="keyword">, </span><span class="string">&apos;ISO-8859-1&apos; </span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>Possible solution #2:
<br>Create a wrapper function and replace htmlspecialchars( to i.e. isohtmlspecialchars( with your IDE/editor/shell...
<br>
<br>Example of a wrapper function: 
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">isohtmlspecialchars</span><span class="keyword">( </span><span class="default">$str </span><span class="keyword">){
<br>&#xA0;&#xA0; return </span><span class="default">htmlspecialchars </span><span class="keyword">( </span><span class="default">$str </span><span class="keyword">, </span><span class="default">ENT_COMPAT </span><span class="keyword">| </span><span class="default">ENT_HTML401 </span><span class="keyword">, </span><span class="string">&apos;ISO-8859-1&apos; </span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems that starting of PHP 5.4 you can not override class method with different signature.<br><br>Example:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">A<br></span><span class="keyword">{ <br>&#xA0; &#xA0; public function </span><span class="default">doSomething</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doSomething</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>PHP Strict standards:&#xA0; Declaration of B::doSomething() should be compatible with A::doSomething(B $a) in Command line code on line 1</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/migration54.incompatible.php)

**[⬆ to root](/)**