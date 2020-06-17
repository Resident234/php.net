# Comparison of floating point numbers




<div class="phpcode"><span class="html">
I couldn&apos;t find much info on stacking the new ternary operator, so I ran some tests:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">1 </span><span class="keyword">?: </span><span class="default">2 </span><span class="keyword">?: </span><span class="default">3</span><span class="keyword">; </span><span class="comment">//1<br></span><span class="keyword">echo </span><span class="default">1 </span><span class="keyword">?: </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">3 </span><span class="keyword">?: </span><span class="default">2</span><span class="keyword">; </span><span class="comment">//1<br></span><span class="keyword">echo </span><span class="default">2 </span><span class="keyword">?: </span><span class="default">1 </span><span class="keyword">?: </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">3</span><span class="keyword">; </span><span class="comment">//2<br></span><span class="keyword">echo </span><span class="default">3 </span><span class="keyword">?: </span><span class="default">2 </span><span class="keyword">?: </span><span class="default">1 </span><span class="keyword">?: </span><span class="default">0</span><span class="keyword">; </span><span class="comment">//3<br><br></span><span class="keyword">echo </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">1 </span><span class="keyword">?: </span><span class="default">2 </span><span class="keyword">?: </span><span class="default">3</span><span class="keyword">; </span><span class="comment">//1<br></span><span class="keyword">echo </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">2 </span><span class="keyword">?: </span><span class="default">3</span><span class="keyword">; </span><span class="comment">//2<br></span><span class="keyword">echo </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">0 </span><span class="keyword">?: </span><span class="default">3</span><span class="keyword">; </span><span class="comment">//3<br></span><span class="default">?&gt;<br></span><br>It works just as expected, returning the first non-false value within a group of expressions.</span>
</div>
  

#


<div class="phpcode"><span class="html">
[Editor&apos;s note: consider using ===]
<br>
<br>I discover after 10 years of PHP development something awfull : even if you make a string comparison (both are strings), strings are tested like integers and leading &quot;space&quot; character (even \n, \r, \t) is ignored ....
<br>
<br>I spent hours because of leading \n in a string ... it hurts my developper sensibility to see two strings beeing compared like integers and not like strings ... I use strcmp now for string comparison ... so stupid ...
<br>
<br>Test code :
<br><span class="default">&lt;?php
<br>
<br>test</span><span class="keyword">(</span><span class="string">&quot;1234&quot;</span><span class="keyword">, </span><span class="string">&quot;1234&quot;</span><span class="keyword">);
<br></span><span class="default">test</span><span class="keyword">(</span><span class="string">&quot;1234&quot;</span><span class="keyword">, </span><span class="string">&quot; 1234&quot;</span><span class="keyword">);
<br></span><span class="default">test</span><span class="keyword">(</span><span class="string">&quot;1234&quot;</span><span class="keyword">, </span><span class="string">&quot;\n1234&quot;</span><span class="keyword">);
<br></span><span class="default">test</span><span class="keyword">(</span><span class="string">&quot;1234&quot;</span><span class="keyword">, </span><span class="string">&quot;1234 &quot;</span><span class="keyword">);
<br></span><span class="default">test</span><span class="keyword">(</span><span class="string">&quot;1234&quot;</span><span class="keyword">, </span><span class="string">&quot;1234\n&quot;</span><span class="keyword">);
<br>
<br>function </span><span class="default">test</span><span class="keyword">(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;h1&gt;[&quot;</span><span class="keyword">.</span><span class="default">show_cr</span><span class="keyword">(</span><span class="default">$v1</span><span class="keyword">).</span><span class="string">&quot;] vs [&quot;</span><span class="keyword">.</span><span class="default">show_cr</span><span class="keyword">(</span><span class="default">$v2</span><span class="keyword">).</span><span class="string">&quot;]&lt;/h1&gt;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; echo </span><span class="default">my_var_dump</span><span class="keyword">(</span><span class="default">$v1</span><span class="keyword">).</span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; echo </span><span class="default">my_var_dump</span><span class="keyword">(</span><span class="default">$v2</span><span class="keyword">).</span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; if(</span><span class="default">$v1 </span><span class="keyword">== </span><span class="default">$v2</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;EQUAL !&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;DIFFERENT !&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>function </span><span class="default">show_cr</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) {
<br>&#xA0; &#xA0; return </span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\\n&quot;</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">);
<br>}
<br>
<br>function </span><span class="default">my_var_dump</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">ob_start</span><span class="keyword">();
<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$dump </span><span class="keyword">= </span><span class="default">show_cr</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">ob_get_contents</span><span class="keyword">()));
<br>&#xA0; &#xA0; </span><span class="default">ob_end_clean</span><span class="keyword">();
<br>&#xA0; &#xA0; return </span><span class="default">$dump</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Displays this -&gt;
<br>
<br>[1234] vs [1234]
<br>string(4) &quot;1234&quot;
<br>string(4) &quot;1234&quot;
<br>EQUAL !
<br>
<br>[1234] vs [ 1234]
<br>string(4) &quot;1234&quot;
<br>string(5) &quot; 1234&quot;
<br>EQUAL !
<br>
<br>[1234] vs [\n1234]
<br>string(4) &quot;1234&quot;
<br>string(5) &quot;\n1234&quot;
<br>EQUAL !
<br>
<br>[1234] vs [1234 ]
<br>string(4) &quot;1234&quot;
<br>string(5) &quot;1234 &quot;
<br>DIFFERENT !
<br>
<br>[1234] vs [1234\n]
<br>string(4) &quot;1234&quot;
<br>string(5) &quot;1234\n&quot;
<br>DIFFERENT !</span>
</div>
  

#


<div class="phpcode"><span class="html">
note: the behavior below is documented in the appendix K about type comparisons, but since it is somewhat buried i thought i should raise it here for people since it threw me for a loop until i figured it out completely.<br><br>just to clarify a tricky point about the == comparison operator when dealing with strings and numbers:<br><br>(&apos;some string&apos; == 0) returns TRUE<br><br>however, (&apos;123&apos; == 0) returns FALSE<br><br>also note that ((int) &apos;some string&apos;) returns 0<br><br>and ((int) &apos;123&apos;) returns 123<br><br>the behavior makes senes but you must be careful when comparing strings to numbers, e.g. when you&apos;re comparing a request variable which you expect to be numeric. its easy to fall into the trap of:<br><br>if ($_GET[&apos;myvar&apos;]==0) dosomething();<br><br>as this will dosomething() even when $_GET[&apos;myvar&apos;] is &apos;some string&apos; and clearly not the value 0<br><br>i was getting lazy with my types since php vars are so flexible, so be warned to pay attention to the details...</span>
</div>
  

#


<div class="phpcode"><span class="html">
I was interested about the following two uses of the ternary operator (PHP &gt;= 5.3) for using a &quot;default&quot; value if a variable is not set or evaluates to false:<br><br><span class="default">&lt;?php<br></span><span class="keyword">(isset(</span><span class="default">$some_variable</span><span class="keyword">) &amp;&amp; </span><span class="default">$some_variable</span><span class="keyword">) ? </span><span class="default">$some_variable </span><span class="keyword">: </span><span class="string">&apos;default_value&apos;</span><span class="keyword">;<br><br></span><span class="default">$some_variable </span><span class="keyword">?: </span><span class="string">&apos;default_value&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>The second is more readable, but will throw an ERR_NOTICE is $some_variable is not set. Of course, this could be overcome by suppressing the notice using the @ operator.<br><br>Performance-wise, though, comparing 1 million iterations of the three statements<br><br>&#xA0; (isset($foo) &amp;&amp; $foo) ? $foo : &apos;&apos;<br>&#xA0; ($foo) ?: &apos;&apos;<br>&#xA0; (@$foo) ?: &apos;&apos;<br><br>results in the following:<br><br>&#xA0; $foo is NOT SET.<br>&#xA0; &#xA0; [isset] 0.18222403526306<br>&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.57496404647827<br>&#xA0; &#xA0; [@ ?:]&#xA0; 0.64780592918396<br>&#xA0; $foo is NULL.<br>&#xA0; &#xA0; [isset] 0.17995285987854<br>&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.15304207801819<br>&#xA0; &#xA0; [@ ?:]&#xA0; 0.20394206047058<br>&#xA0; $foo is FALSE.<br>&#xA0; &#xA0; [isset] 0.19388508796692<br>&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.15359902381897<br>&#xA0; &#xA0; [@ ?:]&#xA0; 0.20741701126099<br>&#xA0; $foo is TRUE.<br>&#xA0; &#xA0; [isset] 0.17265486717224<br>&#xA0; &#xA0; [?:]&#xA0; &#xA0; 0.11773896217346<br>&#xA0; &#xA0; [@ ?:]&#xA0; 0.16193103790283<br><br>In other words, using the long-form ternary operator with isset($some_variable) is preferable overall if $some_variable may not be set.<br><br>(error_reporting was set to zero for the benchmark, to avoid printing a million notices...)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful when using the ternary operator!<br><br>The following will not evaluate to the expected result:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="string">&quot;a string that has a &quot; </span><span class="keyword">. (</span><span class="default">true</span><span class="keyword">) ? </span><span class="string">&apos;true&apos; </span><span class="keyword">: </span><span class="string">&apos;false&apos; </span><span class="keyword">. </span><span class="string">&quot; condition in. &quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Will print true.<br><br>Instead, use this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="string">&quot;a string that has a &quot; </span><span class="keyword">. ((</span><span class="default">true</span><span class="keyword">) ? </span><span class="string">&apos;true&apos; </span><span class="keyword">: </span><span class="string">&apos;false&apos;</span><span class="keyword">) . </span><span class="string">&quot; condition in. &quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>This will evaluate to the expected result: &quot;a string that has a true condition in. &quot;<br><br>I hope this helps.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful with the &quot;==&quot; operator when both operands are strings:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="string">&apos;123&apos; </span><span class="keyword">== </span><span class="string">&apos;&#xA0; &#xA0; &#xA0;&#xA0; 123&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;1e3&apos; </span><span class="keyword">== </span><span class="string">&apos;1000&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;+74951112233&apos; </span><span class="keyword">== </span><span class="string">&apos;74951112233&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;00000020&apos; </span><span class="keyword">== </span><span class="string">&apos;0000000000000000020&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;0X1D&apos; </span><span class="keyword">== </span><span class="string">&apos;29E0&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;0xafebac&apos; </span><span class="keyword">== </span><span class="string">&apos;11529132&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;0xafebac&apos; </span><span class="keyword">== </span><span class="string">&apos;0XAFEBAC&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;0xeb&apos; </span><span class="keyword">== </span><span class="string">&apos;+235e-0&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;0.235&apos; </span><span class="keyword">== </span><span class="string">&apos;+.235&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;0.2e-10&apos; </span><span class="keyword">== </span><span class="string">&apos;2.0E-11&apos;</span><span class="keyword">); </span><span class="comment">// true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;61529519452809720693702583126814&apos; </span><span class="keyword">== </span><span class="string">&apos;61529519452809720000000000000000&apos;</span><span class="keyword">); </span><span class="comment">// true in php &lt; 5.4.4</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The following contrasts the trinary operator associativity in PHP and Java.&#xA0; The first test would work as expected in Java (evaluates left-to-right, associates right-to-left, like if stmnt), the second in PHP (evaluates and associates left-to-right)<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">echo </span><span class="string">&quot;\n\n######----------- trinary operator associativity\n\n&quot;</span><span class="keyword">;<br><br>function </span><span class="default">trinaryTest</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">){<br><br>&#xA0; &#xA0; </span><span class="default">$bar&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">20<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">? </span><span class="string">&quot;greater than 20&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">: </span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">10<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">? </span><span class="string">&quot;greater than 10&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">: </span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">5<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">? </span><span class="string">&quot;greater than 5&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">: </span><span class="string">&quot;not worthy of consideration&quot;</span><span class="keyword">;&#xA0; &#xA0; <br>&#xA0; &#xA0; echo </span><span class="default">$foo</span><span class="keyword">.</span><span class="string">&quot; =&gt;&#xA0; &quot;</span><span class="keyword">.</span><span class="default">$bar</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>}<br><br>echo </span><span class="string">&quot;----trinaryTest\n\n&quot;</span><span class="keyword">;<br></span><span class="default">trinaryTest</span><span class="keyword">(</span><span class="default">21</span><span class="keyword">);<br></span><span class="default">trinaryTest</span><span class="keyword">(</span><span class="default">11</span><span class="keyword">);<br></span><span class="default">trinaryTest</span><span class="keyword">(</span><span class="default">6</span><span class="keyword">);<br></span><span class="default">trinaryTest</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">);<br><br>function </span><span class="default">trinaryTestParens</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">){<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$bar&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">20<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">? </span><span class="string">&quot;greater than 20&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">: (</span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">10<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">? </span><span class="string">&quot;greater than 10&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">: (</span><span class="default">$foo </span><span class="keyword">&gt; </span><span class="default">5<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">? </span><span class="string">&quot;greater than 5&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">: </span><span class="string">&quot;not worthy of consideration&quot;</span><span class="keyword">));&#xA0; &#xA0; <br>&#xA0; &#xA0; echo </span><span class="default">$foo</span><span class="keyword">.</span><span class="string">&quot; =&gt;&#xA0; &quot;</span><span class="keyword">.</span><span class="default">$bar</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>}<br><br>echo </span><span class="string">&quot;----trinaryTestParens\n\n&quot;</span><span class="keyword">;<br></span><span class="default">trinaryTestParens</span><span class="keyword">(</span><span class="default">21</span><span class="keyword">);<br></span><span class="default">trinaryTestParens</span><span class="keyword">(</span><span class="default">11</span><span class="keyword">);<br></span><span class="default">trinaryTest</span><span class="keyword">(</span><span class="default">6</span><span class="keyword">);<br></span><span class="default">trinaryTestParens</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Output:<br><br>######----------- trinary operator associativity <br><br>----trinaryTest<br><br>21 =&gt;&#xA0; greater than 5<br>11 =&gt;&#xA0; greater than 5<br>6 =&gt;&#xA0; greater than 5<br>4 =&gt;&#xA0; not worthy of consideration<br><br>----trinaryTestParens<br><br>21 =&gt;&#xA0; greater than 20<br>11 =&gt;&#xA0; greater than 10<br>6 =&gt;&#xA0; greater than 5<br>4 =&gt;&#xA0; not worthy of consideration</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you want to use the ?: operator, you should be careful with the precedence.
<br>
<br>Here&apos;s an example of the priority of operators:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">echo </span><span class="string">&apos;Hello, &apos; </span><span class="keyword">. isset(</span><span class="default">$i</span><span class="keyword">) ? </span><span class="string">&apos;my friend: &apos; </span><span class="keyword">. </span><span class="default">$username </span><span class="keyword">. </span><span class="string">&apos;, how are you doing?&apos; </span><span class="keyword">: </span><span class="string">&apos;my guest, &apos; </span><span class="keyword">. </span><span class="default">$guestusername </span><span class="keyword">. </span><span class="string">&apos;, please register&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>This make &quot;&apos;Hello, &apos; . isset($i)&quot; the sentence to evaluate. So, if you think to mix more sentences with the ?: operator, please use always parentheses to force the proper evaluation of the sentence.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">echo </span><span class="string">&apos;Hello, &apos; </span><span class="keyword">. (isset(</span><span class="default">$i</span><span class="keyword">) ? </span><span class="string">&apos;my friend: &apos; </span><span class="keyword">. </span><span class="default">$username </span><span class="keyword">. </span><span class="string">&apos;, how are you doing?&apos; </span><span class="keyword">: </span><span class="string">&apos;my guest, &apos; </span><span class="keyword">. </span><span class="default">$guestusername </span><span class="keyword">. </span><span class="string">&apos;, please register&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>for general rule, if you mix ?: with other sentences, always close it with parentheses.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For converted Perl programmers: use strict comparison operators (===, !==) in place of string comparison operators (eq, ne). Don&apos;t use the simple equality operators (==, !=), because ($a == $b) will return TRUE in many situations where ($a eq $b) would return FALSE.
<br>
<br>For instance...
<br>&quot;mary&quot; == &quot;fred&quot; is FALSE, but
<br>&quot;+010&quot; == &quot;10.0&quot; is TRUE (!)
<br>
<br>In the following examples, none of the strings being compared are identical, but because PHP *can* evaluate them as numbers, it does so, and therefore finds them equal...
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">echo (</span><span class="string">&quot;007&quot; </span><span class="keyword">== </span><span class="string">&quot;7&quot; </span><span class="keyword">? </span><span class="string">&quot;EQUAL&quot; </span><span class="keyword">: </span><span class="string">&quot;not equal&quot;</span><span class="keyword">);
<br></span><span class="comment">// Prints: EQUAL
<br>
<br>// Surrounding the strings with single quotes (&apos;) instead of double
<br>// quotes (&quot;) to ensure the contents aren&apos;t evaluated, and forcing
<br>// string types has no effect.
<br></span><span class="keyword">echo ( (string)</span><span class="string">&apos;0001&apos; </span><span class="keyword">== (string)</span><span class="string">&apos;+1.&apos; </span><span class="keyword">? </span><span class="string">&quot;EQUAL&quot; </span><span class="keyword">: </span><span class="string">&quot;not equal&quot;</span><span class="keyword">);
<br></span><span class="comment">// Prints: EQUAL
<br>
<br>// Including non-digit characters (like leading spaces, &quot;e&quot;, the plus
<br>// or minus sign, period, ...) can still result in this behavior, if
<br>// a string happens to be valid scientific notation.
<br></span><span class="keyword">echo (</span><span class="string">&apos;&#xA0; 131e-2&apos; </span><span class="keyword">== </span><span class="string">&apos;001.3100&apos; </span><span class="keyword">? </span><span class="string">&quot;EQUAL&quot; </span><span class="keyword">: </span><span class="string">&quot;not equal&quot;</span><span class="keyword">);
<br></span><span class="comment">// Prints: EQUAL
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>If you&apos;re comparing passwords (or anything else for which &quot;near&quot; precision isn&apos;t good enough) this confusion could be detrimental. Stick with strict comparisons...
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">// Same examples as above, using === instead of ==
<br>
<br></span><span class="keyword">echo (</span><span class="string">&quot;007&quot; </span><span class="keyword">=== </span><span class="string">&quot;7&quot; </span><span class="keyword">? </span><span class="string">&quot;EQUAL&quot; </span><span class="keyword">: </span><span class="string">&quot;not equal&quot;</span><span class="keyword">);
<br></span><span class="comment">// Prints: not equal
<br>
<br></span><span class="keyword">echo ( (string)</span><span class="string">&apos;0001&apos; </span><span class="keyword">=== (string)</span><span class="string">&apos;+1.&apos; </span><span class="keyword">? </span><span class="string">&quot;EQUAL&quot; </span><span class="keyword">: </span><span class="string">&quot;not equal&quot;</span><span class="keyword">);
<br></span><span class="comment">// Prints: not equal
<br>
<br></span><span class="keyword">echo (</span><span class="string">&apos;&#xA0; 131e-2&apos; </span><span class="keyword">=== </span><span class="string">&apos;001.3100&apos; </span><span class="keyword">? </span><span class="string">&quot;EQUAL&quot; </span><span class="keyword">: </span><span class="string">&quot;not equal&quot;</span><span class="keyword">);
<br></span><span class="comment">// Prints: not equal
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A quick way to do mysql bit comparison in php is to use the special character it stores . e.g<br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$AvailableRequests</span><span class="keyword">[</span><span class="string">&apos;OngoingService&apos;</span><span class="keyword">] == </span><span class="string">&apos;&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;td&gt;Yes&lt;/td&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;td&gt;No&lt;/td&gt;&apos;</span><span class="keyword">;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note: according to the spec, PHP&apos;s comparison operators are not transitive.&#xA0; For example, the following are all true in PHP5:
<br>
<br>&quot;11&quot; &lt; &quot;a&quot; &lt; 2 &lt; &quot;11&quot;
<br>
<br>As a result, the outcome of sorting an array depends on the order the elements appear in the pre-sort array.&#xA0; The following code will dump out two arrays with *different* orderings:
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= array(</span><span class="default">2</span><span class="keyword">,&#xA0; &#xA0; </span><span class="string">&quot;a&quot;</span><span class="keyword">,&#xA0; </span><span class="string">&quot;11&quot;</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);
<br></span><span class="default">$b </span><span class="keyword">= array(</span><span class="default">2</span><span class="keyword">,&#xA0; &#xA0; </span><span class="string">&quot;11&quot;</span><span class="keyword">, </span><span class="string">&quot;a&quot;</span><span class="keyword">,&#xA0; </span><span class="default">2</span><span class="keyword">);
<br></span><span class="default">sort</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);
<br></span><span class="default">sort</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>This is not a bug report -- given the spec on this documentation page, what PHP does is &quot;correct&quot;.&#xA0; But that may not be what was intended...</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware of the consequences of comparing strings to numbers.&#xA0; You can disprove the laws of the universe.<br><br>echo (&apos;X&apos; == 0 &amp;&amp; &apos;X&apos; == true &amp;&amp; 0 == false) ? &apos;true == false&apos; : &apos;sanity prevails&apos;;<br><br>This will output &apos;true == false&apos;.&#xA0; This stems from the use of the UNIX function strtod() to convert strings to numbers before comparing.&#xA0; Since &apos;X&apos; or any other string without a number in it converts to 0 when compared to a number, 0 == 0 &amp;&amp; &apos;X&apos; == true &amp;&amp; 0 == false</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can&apos;t just compare two arrays with the === operator
<br>like you would think to find out if they are equal or not.&#xA0; This is more complicated when you have multi-dimensional arrays.&#xA0; Here is a recursive comparison function.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * Compares two arrays to see if they contain the same values.&#xA0; Returns TRUE or FALSE.
<br> * usefull for determining if a record or block of data was modified (perhaps by user input)
<br> * prior to setting a &quot;date_last_updated&quot; or skipping updating the db in the case of no change.
<br> *
<br> * @param array $a1
<br> * @param array $a2
<br> * @return boolean
<br> */
<br></span><span class="keyword">function </span><span class="default">array_compare_recursive</span><span class="keyword">(</span><span class="default">$a1</span><span class="keyword">, </span><span class="default">$a2</span><span class="keyword">)
<br>{
<br>&#xA0;&#xA0; if (!(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$a1</span><span class="keyword">) and (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$a2</span><span class="keyword">)))) { return </span><span class="default">FALSE</span><span class="keyword">;}&#xA0; &#xA0; 
<br>&#xA0; &#xA0; 
<br>&#xA0;&#xA0; if (!</span><span class="default">count</span><span class="keyword">(</span><span class="default">$a1</span><span class="keyword">) == </span><span class="default">count</span><span class="keyword">(</span><span class="default">$a2</span><span class="keyword">)) 
<br>&#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">FALSE</span><span class="keyword">; </span><span class="comment">// arrays don&apos;t have same number of entries
<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; &#xA0; 
<br>&#xA0;&#xA0; foreach (</span><span class="default">$a1 </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">) 
<br>&#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0;&#xA0; if (!</span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$a2</span><span class="keyword">)) 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {return </span><span class="default">FALSE</span><span class="keyword">; </span><span class="comment">// uncomparable array keys don&apos;t match
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">} 
<br>&#xA0; &#xA0; &#xA0;&#xA0; elseif (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) and </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$a2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]))&#xA0; </span><span class="comment">// if both entries are arrays then compare recursive 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">{if (!</span><span class="default">array_compare_recursive</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">,</span><span class="default">$a2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])) return </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } 
<br>&#xA0; &#xA0; &#xA0;&#xA0; elseif (!(</span><span class="default">$val </span><span class="keyword">=== </span><span class="default">$a2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])) </span><span class="comment">// compare entries must be of same type.
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">{return </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
<br>&#xA0;&#xA0; }
<br>&#xA0;&#xA0; return </span><span class="default">TRUE</span><span class="keyword">; </span><span class="comment">// $a1 === $a2
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I think everybody should read carefully what &quot;jeronimo at DELETE_THIS dot transartmedia dot com&quot; wrote. It&apos;s a great pitfall even for seasoned programmers and should be looked upon with a great attention.<br>For example, comparing passwords with == may result in a very large security hole.<br><br>I would add some more to it:<br><br>The workaround is to use strcmp() or ===.<br><br>Note on ===:<br><br>While the php documentation says that, basically,<br>($a===$b)&#xA0; is the same as&#xA0; ($a==$b &amp;&amp; gettype($a) == gettype($b)),<br>this is not true.<br><br>The difference between == and === is that === never does any type conversion. So, while, according to documentation, (&quot;+0.1&quot; === &quot;.1&quot;) should return true (because both are strings and == returns true), === actually returns false (which is good).</span>
</div>
  

#


<div class="phpcode"><span class="html">
beware of the fact, that there is no `&lt;==` nor `&gt;==` therefore `false &lt;= 0` will be `true`. php v. 5.4.27</span>
</div>
  

#


<div class="phpcode"><span class="html">
When you want to know if two arrays contain the same values, regardless of the values&apos; order, you cannot use &quot;==&quot; or &quot;===&quot;.&#xA0; In other words:<br><br><span class="default">&lt;?php<br></span><span class="keyword">(array(</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">) == array(</span><span class="default">2</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">)) === </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>To answer that question, use:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_equal</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) {<br>&#xA0; &#xA0; return (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">) &amp;&amp; </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">) &amp;&amp; </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) === </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;<br></span><br>A related, but more strict problem, is if you need to ensure that two arrays contain the same key=&gt;value pairs, regardless of the order of the pairs.&#xA0; In that case, use:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_identical</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) {<br>&#xA0; &#xA0; return (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">) &amp;&amp; </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">) &amp;&amp; </span><span class="default">array_diff_assoc</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) === </span><span class="default">array_diff_assoc</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">));<br>}<br></span><span class="default">?&gt;<br></span><br>Example:<br><span class="default">&lt;?php<br>$a </span><span class="keyword">= array (</span><span class="default">2</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= array (</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);<br></span><span class="comment">// true === array_equal($a, $b);<br>// false === array_identical($a, $b);<br><br></span><span class="default">$a </span><span class="keyword">= array (</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= array (</span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">);<br></span><span class="comment">// true === array_identical($a, $b)<br>// true === array_equal($a, $b)<br></span><span class="default">?&gt;<br></span><br>(See also the solution &quot;rshawiii at yahoo dot com&quot; posted)</span>
</div>
  

#


<div class="phpcode"><span class="html">
In the table &quot;Comparison with Various Types&quot;, please move the last line about &quot;Object&quot; to be above the line about &quot;Array&quot;, since Object is considered to be greater than Array (tested on 5.3.3)<br><br>(Please remove my &quot;Anonymous&quot; post of the same content before. You could check IP to see that I forgot to type my name)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that typecasting will NOT prevent the default behavior for converting two numeric strings to numbers when comparing them.
<br>
<br>e.g.:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if ((string) </span><span class="string">&apos;0123&apos; </span><span class="keyword">== (string) </span><span class="string">&apos;123&apos;</span><span class="keyword">)
<br>&#xA0; &#xA0; print </span><span class="string">&apos;equals&apos;</span><span class="keyword">;
<br>else
<br>&#xA0; &#xA0; print </span><span class="string">&apos;doesn\&apos;t equal&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Still prints &apos;equals&apos;
<br>
<br>As far as I can tell the only way to avoid this is to use the identity comparison operators (=== and !==).</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note: The ternary shortcut currently seems to be of no use in dealing with unexisting keys in an array, as PHP will throw an error. Take the following example.<br><br><span class="default">&lt;?php<br>$_POST</span><span class="keyword">[</span><span class="string">&apos;Unexisting&apos;</span><span class="keyword">] = </span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;Unexisting&apos;</span><span class="keyword">] ?: </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>PHP will throw an error that the &quot;Unexisting&quot; key does not exist. The @ operator does not work here to suppress this error.</span>
</div>
  

#


<div class="phpcode"><span class="html">
a function to help settings default values, it returns its own first non-empty argument :<br><br>make your own eor combos !<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/*<br> * Either Or<br> *<br> * usage:&#xA0; $foo = eor(test1(),test2(),&quot;default&quot;);<br> * usage:&#xA0; $foo = eor($_GET[&apos;foo&apos;], foogen(), $foo, &quot;bar&quot;);<br> */<br><br></span><span class="keyword">function </span><span class="default">eor</span><span class="keyword">() {<br>&#xA0; &#xA0; </span><span class="default">$vars </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">();<br>&#xA0; &#xA0;&#xA0; while (!empty(</span><span class="default">$vars</span><span class="keyword">) &amp;&amp; empty(</span><span class="default">$defval</span><span class="keyword">))&#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$defval </span><span class="keyword">= </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$vars</span><span class="keyword">);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0;&#xA0; return </span><span class="default">$defval</span><span class="keyword">;<br>}<br><br> <br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need nested ifs on I var its important to group the if so it works.<br>Example:<br><span class="default">&lt;?php<br></span><span class="comment">//Dont Works<br>//Parse error: parse error, unexpected &apos;:&apos; <br> </span><span class="default">$var</span><span class="keyword">=</span><span class="string">&apos;&lt;option value=&quot;1&quot; &apos;</span><span class="keyword">.</span><span class="default">$status </span><span class="keyword">== </span><span class="string">&quot;1&quot; </span><span class="keyword">? </span><span class="string">&apos;selected=&quot;selected&quot;&apos; </span><span class="keyword">:</span><span class="string">&apos;&apos;</span><span class="keyword">.</span><span class="string">&apos;&gt;Value 1&lt;/option&gt;&apos;</span><span class="keyword">;<br> </span><span class="comment">//Works:<br> </span><span class="default">$var</span><span class="keyword">=</span><span class="string">&apos;&lt;option value=&quot;1&quot; &apos;</span><span class="keyword">.(</span><span class="default">$status </span><span class="keyword">== </span><span class="string">&quot;1&quot; </span><span class="keyword">? </span><span class="string">&apos;selected=&quot;selected&quot;&apos; </span><span class="keyword">:</span><span class="string">&apos;&apos;</span><span class="keyword">).</span><span class="string">&apos;&gt;Value 1&lt;/option&gt;&apos;</span><span class="keyword">;<br><br>echo </span><span class="default">$var</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that the &quot;ternary operator&quot; is better described as the &quot;conditional operator&quot;. The former name merely notes that it has three arguments without saying anything about what it does. Needless to say, if PHP picked up any more ternary operators, this will be a problem.<br><br>&quot;Conditional Operator&quot; is actually descriptive of the semantics, and is the name historically given to it in, e.g., C.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I came across peculiar outputs while I was attempting to debug a script
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment"># Setup platform (pre conditions somewhere in a loop)
<br></span><span class="default">$index</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$tally </span><span class="keyword">= array();
<br>
<br></span><span class="comment"># May work with warnings that $tally[$index] is not initialized
<br># Notice: Undefined offset: 1 in D:\htdocs\colors\ColorCompare\i.php on line #__
<br># It is an old fashioned way.
<br># $tally[$index] = $tally[$index] + 1;
<br>
<br># Does not work: Loops to attempt to change $index and values are aways unaffected
<br></span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">] = isset(</span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">])?</span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]:</span><span class="default">0</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">] = isset(</span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">])?</span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]:</span><span class="default">0</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">] = isset(</span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">])?</span><span class="default">$tally</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]:</span><span class="default">0</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">;
<br></span><span class="comment">/*
<br># These three lines output:
<br>Array
<br>(
<br>&#xA0; &#xA0; [1] =&gt; 1
<br>)
<br>*/
<br>
<br># Works: This is what I need/expect
<br># $tally[$index] = 1+(isset($tally[$index])?$tally[$index]:0);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$tally</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>The second block obviously does not work what one expects.
<br>Third part is good.</span>
</div>
  

#


<div class="phpcode"><span class="html">
With Nested ternary Operators you have to set the logical&#xA0; parentheses to get the correct result.<br><br><span class="default">&lt;?php<br>$test</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">;<br></span><span class="default">$test2</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">;<br><br>(</span><span class="default">$test</span><span class="keyword">) ? </span><span class="string">&quot;TEST1 true&quot; </span><span class="keyword">:&#xA0; (</span><span class="default">$test2</span><span class="keyword">) ? </span><span class="string">&quot;TEST2 true&quot; </span><span class="keyword">: </span><span class="string">&quot;false&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>This will output: TEST2 true;<br><br>correct:<br><br><span class="default">&lt;?php<br>$test</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">;<br></span><span class="default">$test2</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">;<br><br>(</span><span class="default">$test</span><span class="keyword">) ? </span><span class="string">&quot;TEST1 true&quot; </span><span class="keyword">: ((</span><span class="default">$test2</span><span class="keyword">) ? </span><span class="string">&quot;TEST2 true&quot; </span><span class="keyword">: </span><span class="string">&quot;false&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Anyway don&apos;t nest them to much....!!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.comparison.php)

**[To root](/README.md)**