# Magic Methods




<div class="phpcode"><span class="html">
The __toString() method is extremely useful for converting class attribute names and values into common string representations of data (of which there are many choices). I mention this as previous references to __toString() refer only to debugging uses.<br><br>I have previously used the __toString() method in the following ways:<br><br> - representing a data-holding object as:<br>&#xA0;&#xA0; - XML<br>&#xA0;&#xA0; - raw POST data<br>&#xA0;&#xA0; - a GET query string<br>&#xA0;&#xA0; - header name:value pairs<br><br> - representing a custom mail object as an actual email (headers then body, all correctly represented)<br><br>When creating a class, consider what possible standard string representations are available and, of those, which would be the most relevant with respect to the purpose of the class.<br><br>Being able to represent data-holding objects in standardised string forms makes it much easier for your internal representations of data to be shared in an interoperable way with other applications.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be very careful to define __set_state() in classes which inherit from a parent using it, as the static __set_state() call will be called for any children.&#xA0; If you are not careful, you will end up with an object of the wrong type.&#xA0; Here is an example:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">A
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; public </span><span class="default">$var1</span><span class="keyword">; 
<br>
<br>&#xA0; &#xA0; public static function </span><span class="default">__set_state</span><span class="keyword">(</span><span class="default">$an_array</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">A</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">var1 </span><span class="keyword">= </span><span class="default">$an_array</span><span class="keyword">[</span><span class="string">&apos;var1&apos;</span><span class="keyword">];&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$obj</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A </span><span class="keyword">{
<br>}
<br>
<br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">B</span><span class="keyword">;
<br></span><span class="default">$b</span><span class="keyword">-&gt;</span><span class="default">var1 </span><span class="keyword">= </span><span class="default">5</span><span class="keyword">;
<br>
<br>eval(</span><span class="string">&apos;$new_b = &apos; </span><span class="keyword">. </span><span class="default">var_export</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">) . </span><span class="string">&apos;;&apos;</span><span class="keyword">); 
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$new_b</span><span class="keyword">);
<br></span><span class="comment">/*
<br>object(A)#2 (1) {
<br>&#xA0; [&quot;var1&quot;]=&gt;
<br>&#xA0; int(5)
<br>}
<br>*/
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Ever wondered why you can&apos;t throw exceptions from __toString()? Yeah me too. <br><br>Well now you can! This trick allows you to throw any type of exception from within a __toString(), with a full &amp; correct backtrace.<br><br>How does it work? Well PHP __toString() handling is not as strict in every case: throwing an Exception from __toString() triggers a fatal E_ERROR, but returning a non-string value from a __toString() triggers a non-fatal E_RECOVERABLE_ERROR. <br>Add a little bookkeeping, and can circumvented this PHP deficiency!<br>(tested to work PHP 5.3+)<br><br><span class="default">&lt;?php<br><br>set_error_handler</span><span class="keyword">(array(</span><span class="string">&apos;My_ToStringFixer&apos;</span><span class="keyword">, </span><span class="string">&apos;errorHandler&apos;</span><span class="keyword">));<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL </span><span class="keyword">| </span><span class="default">E_STRICT</span><span class="keyword">);<br><br>class </span><span class="default">My_ToStringFixer<br></span><span class="keyword">{<br>&#xA0; &#xA0; protected static </span><span class="default">$_toStringException</span><span class="keyword">;<br><br>&#xA0; &#xA0; public static function </span><span class="default">errorHandler</span><span class="keyword">(</span><span class="default">$errorNumber</span><span class="keyword">, </span><span class="default">$errorMessage</span><span class="keyword">, </span><span class="default">$errorFile</span><span class="keyword">, </span><span class="default">$errorLine</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">self</span><span class="keyword">::</span><span class="default">$_toStringException</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$exception </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">$_toStringException</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Always unset &apos;_toStringException&apos;, we don&apos;t want a straggler to be found later if something came between the setting and the error<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$_toStringException </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;~^Method .*::__toString\(\) must return a string value$~&apos;</span><span class="keyword">, </span><span class="default">$errorMessage</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw </span><span class="default">$exception</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public static function </span><span class="default">throwToStringException</span><span class="keyword">(</span><span class="default">$exception</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Should not occur with prescribed usage, but in case of recursion: clean out exception, return a valid string, and weep<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (isset(</span><span class="default">self</span><span class="keyword">::</span><span class="default">$_toStringException</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$_toStringException </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$_toStringException </span><span class="keyword">= </span><span class="default">$exception</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">null</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">My_Class<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">doComplexStuff</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Oh noes!&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">__toString</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; try<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// do your complex thing which might trigger an exception<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">doComplexStuff</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; catch (</span><span class="default">Exception $e</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// The &apos;return&apos; is required to trigger the trick<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">My_ToStringFixer</span><span class="keyword">::</span><span class="default">throwToStringException</span><span class="keyword">(</span><span class="default">$e</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$x </span><span class="keyword">= new </span><span class="default">My_Class</span><span class="keyword">();<br><br>try<br>{<br>&#xA0; &#xA0; echo </span><span class="default">$x</span><span class="keyword">;<br>}<br>catch (</span><span class="default">Exception $e</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Caught Exception! : &apos;</span><span class="keyword">. </span><span class="default">$e</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.magic.php)

**[To root](/README.md)**