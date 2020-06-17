# Alternative syntax for control structures




<div class="phpcode"><span class="html">
It seems to me, that many people think that<br><br><span class="default">&lt;?php </span><span class="keyword">if (</span><span class="default">$a </span><span class="keyword">== </span><span class="default">5</span><span class="keyword">): </span><span class="default">?&gt;<br></span>A ist gleich 5<br><span class="default">&lt;?php </span><span class="keyword">endif; </span><span class="default">?&gt;<br></span><br>is only with alternate syntax possible, but <br><br><span class="default">&lt;?php </span><span class="keyword">if (</span><span class="default">$a </span><span class="keyword">== </span><span class="default">5</span><span class="keyword">){ </span><span class="default">?&gt;<br></span>A ist gleich 5<br><span class="default">&lt;?php </span><span class="keyword">}; </span><span class="default">?&gt;<br></span><br>is also possible.<br><br>alternate syntax makes the code only clearer and easyer to read</span>
</div>
  

#


<div class="phpcode"><span class="html">
A simple alternative to an if statement, which is almost like a ternary operator, is the use of AND. Consider the following:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0;&#xA0; $value </span><span class="keyword">= </span><span class="string">&apos;Jesus&apos;</span><span class="keyword">;<br><br>&#xA0; &#xA0;&#xA0; </span><span class="comment">// This is a simple if statement<br>&#xA0; &#xA0;&#xA0; </span><span class="keyword">if( isset( </span><span class="default">$value </span><span class="keyword">) )<br>&#xA0; &#xA0;&#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0;&#xA0; }<br><br>&#xA0; &#xA0;&#xA0; print </span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;<br><br>&#xA0; &#xA0;&#xA0; </span><span class="comment">// This is an alternative<br>&#xA0; &#xA0;&#xA0; </span><span class="keyword">isset( </span><span class="default">$value </span><span class="keyword">) AND print( </span><span class="default">$value </span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This does not work with echo() for some reason. I find this extremely useful!</span>
</div>
  

#


<div class="phpcode"><span class="html">
isset( $value ) AND print( $value );<br><br>the reason why it doesn&apos;t work with echo, it&apos;s because echo does not return anything, while print _always_ returns 1, which is considered true in the expression</span>
</div>
  

#


<div class="phpcode"><span class="html">
Consider the following hypothetical PHP example:<br><br><span class="default">&lt;?php<br>$bar </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br></span><span class="default">$foo </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br><br>if (isset(</span><span class="default">$bar</span><span class="keyword">)):<br>&#xA0;&#xA0; if (isset(</span><span class="default">$foo</span><span class="keyword">)) echo </span><span class="string">&quot;Both are set.&quot;</span><span class="keyword">;<br>elseif (isset(</span><span class="default">$foo</span><span class="keyword">)):<br>&#xA0;&#xA0; echo </span><span class="string">&quot;Only &apos;foo&apos; is set.&quot;</span><span class="keyword">;<br>else:<br>&#xA0;&#xA0; echo </span><span class="string">&quot;Only &apos;bar&apos; is set.&quot;</span><span class="keyword">;<br>endif;<br></span><span class="default">?&gt;<br></span><br>Disconsider the dumb logic and focus on the elseif line. If you try it yourself you will get a PHP EXCEPTION error saying: syntax error, unexpected &apos;:&apos; .<br><br>Now, you may think the fix is to have the sub-if enclosed in between { } instead of being a single line statement, like this:<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br></span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br><br>if (isset(</span><span class="default">$bar</span><span class="keyword">)):<br>&#xA0;&#xA0; if (isset(</span><span class="default">$foo</span><span class="keyword">)) {<br>&#xA0; &#xA0;&#xA0; echo </span><span class="string">&quot;Both are set.&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; }<br>elseif (isset(</span><span class="default">$foo</span><span class="keyword">)):<br>&#xA0;&#xA0; echo </span><span class="string">&quot;Only &apos;foo&apos; is set.&quot;</span><span class="keyword">;<br>else:<br>&#xA0;&#xA0; echo </span><span class="string">&quot;Only &apos;bar&apos; is set.&quot;</span><span class="keyword">;<br>endif;<br></span><span class="default">?&gt;<br></span><br>Wrong! The error remains. Exactly the same EXCEPTION as before...<br>&#xA0; &#xA0; <br>Well, here is what I found: if you put a semicolon (;) AFTER the curly bracket (}) which resides immediately before the elseif statement, then the error is gone! Try it:<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br></span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br><br>if (isset(</span><span class="default">$bar</span><span class="keyword">)):<br>&#xA0;&#xA0; if (isset(</span><span class="default">$foo</span><span class="keyword">)) {<br>&#xA0; &#xA0;&#xA0; echo </span><span class="string">&quot;Both are set.&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; };<br>elseif (isset(</span><span class="default">$foo</span><span class="keyword">)):<br>&#xA0;&#xA0; echo </span><span class="string">&quot;Only &apos;foo&apos; is set.&quot;</span><span class="keyword">;<br>else:<br>&#xA0;&#xA0; echo </span><span class="string">&quot;Only &apos;bar&apos; is set.&quot;</span><span class="keyword">;<br>endif;<br></span><span class="default">?&gt;<br></span><br>Weird enough, if you go back to the first example and DOUBLE the semicolon immediately before the elseif statement, it will also work:<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br></span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br><br>if (isset(</span><span class="default">$bar</span><span class="keyword">)):<br>&#xA0; if (isset(</span><span class="default">$foo</span><span class="keyword">)) echo </span><span class="string">&quot;Both are set.&quot;</span><span class="keyword">;;<br>elseif (isset(</span><span class="default">$foo</span><span class="keyword">)):<br>&#xA0; echo </span><span class="string">&quot;Only &apos;foo&apos; is set.&quot;</span><span class="keyword">;<br>else:<br>&#xA0; echo </span><span class="string">&quot;Only &apos;bar&apos; is set.&quot;</span><span class="keyword">;<br>endif;<br></span><span class="default">?&gt;<br></span><br>But, it doesn&apos;t end there. You can also do this:<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= </span><span class="string">&apos;foo&apos;</span><span class="keyword">;<br></span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br><br>if (isset(</span><span class="default">$bar</span><span class="keyword">)):<br>&#xA0; if (isset(</span><span class="default">$foo</span><span class="keyword">)): echo </span><span class="string">&quot;Both are set.&quot;</span><span class="keyword">;<br>elseif (isset(</span><span class="default">$foo</span><span class="keyword">)):<br>&#xA0; echo </span><span class="string">&quot;Only &apos;foo&apos; is set.&quot;</span><span class="keyword">;<br>else:<br>&#xA0; echo </span><span class="string">&quot;Only &apos;bar&apos; is set.&quot;</span><span class="keyword">;<br>endif;<br></span><span class="default">?&gt;<br></span><br>However, in this last example, the logic gets totally scrambled! The elseif will now belong to the sub-if instead of the first if, and the rest of the logic will all behave as a &quot;one single statement&quot; in response to the first if only. Very confusing and error prone (be careful).<br><br>The differences are very subtle and can deceive the eyes (especially while debugging). For this reason, I strongly suggest the first example from this answer: when using IF-ELSEIF blocks (AKA &quot;Alternative Syntax&quot;), if another IF is required inside it, enclose it in between {} and don&apos;t forget to add a semicolon after the last }. Example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (isset(</span><span class="default">$bar</span><span class="keyword">)):<br>&#xA0;&#xA0; if (isset(</span><span class="default">$foo</span><span class="keyword">)) {<br>&#xA0; &#xA0;&#xA0; echo </span><span class="string">&quot;Both are set.&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; };<br>elseif (...):<br></span><span class="default">?&gt;<br></span><br>Maybe the truth is that someone screwed up in the language parsing process for those PHP Block Alternative Statements or failed to document this very important detail!</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you wan&apos;t to use the alternative syntax for switch statements this won&apos;t work:<br><br>&lt;div&gt;<br><span class="default">&lt;?php </span><span class="keyword">switch(</span><span class="default">$variable</span><span class="keyword">): </span><span class="default">?&gt;<br>&lt;?php </span><span class="keyword">case </span><span class="default">1</span><span class="keyword">: </span><span class="default">?&gt;<br></span>&lt;div&gt;<br>Newspage<br>&lt;/div&gt;<br><span class="default">&lt;?php </span><span class="keyword">break;</span><span class="default">?&gt;<br>&lt;?php </span><span class="keyword">case </span><span class="default">2</span><span class="keyword">: </span><span class="default">?&gt;<br></span>&lt;/div&gt;<br>Forum<br>&lt;div&gt;<br><span class="default">&lt;?php </span><span class="keyword">break;</span><span class="default">?&gt;<br>&lt;?php </span><span class="keyword">endswitch;</span><span class="default">?&gt;<br></span>&lt;/div&gt;<br><br>Instead you have to workaround like this:<br><br>&lt;div&gt;<br><span class="default">&lt;?php </span><span class="keyword">switch(</span><span class="default">$variable</span><span class="keyword">): <br>case </span><span class="default">1</span><span class="keyword">: </span><span class="default">?&gt;<br></span>&lt;div&gt;<br>Newspage<br>&lt;/div&gt;<br><span class="default">&lt;?php </span><span class="keyword">break;</span><span class="default">?&gt;<br>&lt;?php </span><span class="keyword">case </span><span class="default">2</span><span class="keyword">: </span><span class="default">?&gt;<br></span>&lt;/div&gt;<br>Forum<br>&lt;div&gt;<br><span class="default">&lt;?php </span><span class="keyword">break;</span><span class="default">?&gt;<br>&lt;?php </span><span class="keyword">endswitch;</span><span class="default">?&gt;<br></span>&lt;/div&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.alternative-syntax.php)

**[To root](/README.md)**