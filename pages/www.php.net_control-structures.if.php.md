# if




<div class="phpcode"><span class="html">
easy way to execute conditional html / javascript / css / other language code with php if else:<br><br><span class="default">&lt;?php </span><span class="keyword">if (</span><span class="default">condition</span><span class="keyword">): </span><span class="default">?&gt;<br></span><br>html code to run if condition is true<br><br><span class="default">&lt;?php </span><span class="keyword">else: </span><span class="default">?&gt;<br></span><br>html code to run if condition is false<br><br><span class="default">&lt;?php </span><span class="keyword">endif </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Any variables defined inside the if block will be available outside the block. Remember that the if doesn&apos;t have its own scope.<br><br><span class="default">&lt;?php<br>$bool </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>if (</span><span class="default">$bool</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$hi </span><span class="keyword">= </span><span class="string">&apos;Hello to all people!&apos;</span><span class="keyword">;<br>}<br>echo </span><span class="default">$hi</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>It will print &apos;Hello to all people!&apos;<br><br>On the other hand, this will have no output:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$hi </span><span class="keyword">= </span><span class="string">&apos;Hello to all people!&apos;</span><span class="keyword">;<br>}<br>echo </span><span class="default">$hi</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
re: #80305<br><br>Again useful for newbies:<br><br>if you need to compare a variable with a value, instead of doing<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">$foo </span><span class="keyword">== </span><span class="default">3</span><span class="keyword">) </span><span class="default">bar</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>do<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">3 </span><span class="keyword">== </span><span class="default">$foo</span><span class="keyword">) </span><span class="default">bar</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>this way, if you forget a =, it will become<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">3 </span><span class="keyword">= </span><span class="default">$foo</span><span class="keyword">) </span><span class="default">bar</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>and PHP will report an error.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can have &apos;nested&apos; if statements withing a single if statement, using additional parenthesis.
<br>For example, instead of having:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if( </span><span class="default">$a </span><span class="keyword">== </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">$a </span><span class="keyword">== </span><span class="default">2 </span><span class="keyword">) {
<br>&#xA0; &#xA0; if( </span><span class="default">$b </span><span class="keyword">== </span><span class="default">3 </span><span class="keyword">|| </span><span class="default">$b </span><span class="keyword">== </span><span class="default">4 </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if( </span><span class="default">$c </span><span class="keyword">== </span><span class="default">5 </span><span class="keyword">|| $ </span><span class="default">d </span><span class="keyword">== </span><span class="default">6 </span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">//Do something here.
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>You could just simply do this:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if( (</span><span class="default">$a</span><span class="keyword">==</span><span class="default">1 </span><span class="keyword">|| </span><span class="default">$a</span><span class="keyword">==</span><span class="default">2</span><span class="keyword">) &amp;&amp; (</span><span class="default">$b</span><span class="keyword">==</span><span class="default">3 </span><span class="keyword">|| </span><span class="default">$b</span><span class="keyword">==</span><span class="default">4</span><span class="keyword">) &amp;&amp; (</span><span class="default">$c</span><span class="keyword">==</span><span class="default">5 </span><span class="keyword">|| </span><span class="default">$c</span><span class="keyword">==</span><span class="default">6</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; </span><span class="comment">//do that something here.
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this helps!</span>
</div>
  

#


<div class="phpcode"><span class="html">
An other way for controls is the ternary operator (see Comparison Operators) that can be used as follows:<br><br><span class="default">&lt;?php<br>$v </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br><br></span><span class="default">$r </span><span class="keyword">= (</span><span class="default">1 </span><span class="keyword">== </span><span class="default">$v</span><span class="keyword">) ? </span><span class="string">&apos;Yes&apos; </span><span class="keyword">: </span><span class="string">&apos;No&apos;</span><span class="keyword">; </span><span class="comment">// $r is set to &apos;Yes&apos;<br></span><span class="default">$r </span><span class="keyword">= (</span><span class="default">3 </span><span class="keyword">== </span><span class="default">$v</span><span class="keyword">) ? </span><span class="string">&apos;Yes&apos; </span><span class="keyword">: </span><span class="string">&apos;No&apos;</span><span class="keyword">; </span><span class="comment">// $r is set to &apos;No&apos;<br><br></span><span class="keyword">echo (</span><span class="default">1 </span><span class="keyword">== </span><span class="default">$v</span><span class="keyword">) ? </span><span class="string">&apos;Yes&apos; </span><span class="keyword">: </span><span class="string">&apos;No&apos;</span><span class="keyword">; </span><span class="comment">// &apos;Yes&apos; will be printed<br><br>// and since PHP 5.3<br></span><span class="default">$v </span><span class="keyword">= </span><span class="string">&apos;My Value&apos;</span><span class="keyword">;<br></span><span class="default">$r </span><span class="keyword">= (</span><span class="default">$v</span><span class="keyword">) ?: </span><span class="string">&apos;No Value&apos;</span><span class="keyword">; </span><span class="comment">// $r is set to &apos;My Value&apos; because $v is evaluated to TRUE<br><br></span><span class="default">$v </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>echo (</span><span class="default">$v</span><span class="keyword">) ?: </span><span class="string">&apos;No Value&apos;</span><span class="keyword">; </span><span class="comment">// &apos;No Value&apos; will be printed because $v is evaluated to FALSE<br></span><span class="default">?&gt;<br></span><br>Parentheses can be left out in all examples above.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In addition to the traditional syntax for if (condition) action;<br>I am fond of the ternary operator that does the same thing, but with fewer words and code to type:<br><br>(condition ? action_if_true: action_if_false;)<br><br>example<br><br>(x &gt; y? &apos;Passed the test&apos; : &apos;Failed the test&apos;)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.if.php)

**[To root](/README.md)**