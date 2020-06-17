# foreach




<div class="phpcode"><span class="html">
You can also use the alternative syntax for the foreach cycle:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$element</span><span class="keyword">):
<br>&#xA0; </span><span class="comment">#do something
<br></span><span class="keyword">endforeach;
<br></span><span class="default">?&gt;
<br></span>
<br>Just thought it worth mentioning.</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;Reference of a $value and the last array element remain even after the foreach loop. It is recommended to destroy it by unset().&quot;<br><br>I cannot stress this point of the documentation enough! Here is a simple example of exactly why this must be done:<br><br><span class="default">&lt;?php<br>$arr1 </span><span class="keyword">= array(</span><span class="string">&quot;a&quot; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&quot;b&quot; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&quot;c&quot; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">);<br></span><span class="default">$arr2 </span><span class="keyword">= array(</span><span class="string">&quot;x&quot; </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">, </span><span class="string">&quot;y&quot; </span><span class="keyword">=&gt; </span><span class="default">5</span><span class="keyword">, </span><span class="string">&quot;z&quot; </span><span class="keyword">=&gt; </span><span class="default">6</span><span class="keyword">);<br><br>foreach (</span><span class="default">$arr1 </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; &amp;</span><span class="default">$val</span><span class="keyword">) {}<br>foreach (</span><span class="default">$arr2 </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">) {}<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$arr1</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$arr2</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The output is:<br>array(3) { [&quot;a&quot;]=&gt; int(1) [&quot;b&quot;]=&gt; int(2) [&quot;c&quot;]=&gt; &amp;int(6) }<br>array(3) { [&quot;x&quot;]=&gt; int(4) [&quot;y&quot;]=&gt; int(5) [&quot;z&quot;]=&gt; int(6) }<br><br>Notice how the last index in $arr1 is now the value from the last index in $arr2!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Even though it is not mentioned in this article, you can use &quot;break&quot; control structure to exit from the &quot;foreach&quot; loop.<br><br><span class="default">&lt;?php<br><br>$array </span><span class="keyword">= [ </span><span class="string">&apos;one&apos;</span><span class="keyword">, </span><span class="string">&apos;two&apos;</span><span class="keyword">, </span><span class="string">&apos;three&apos;</span><span class="keyword">, </span><span class="string">&apos;four&apos;</span><span class="keyword">, </span><span class="string">&apos;five&apos; </span><span class="keyword">];<br><br>foreach( </span><span class="default">$array </span><span class="keyword">as </span><span class="default">$value </span><span class="keyword">){<br>&#xA0; &#xA0; if( </span><span class="default">$value </span><span class="keyword">== </span><span class="string">&apos;three&apos; </span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Number three was found!&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
foreach and the while/list/each methods are not completely identical, and there are occasions where one way is beneficial over the other.
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">,</span><span class="default">5</span><span class="keyword">,</span><span class="default">6</span><span class="keyword">,</span><span class="default">7</span><span class="keyword">,</span><span class="default">8</span><span class="keyword">,</span><span class="default">9</span><span class="keyword">);
<br>
<br>foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; unset(</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$key </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">]);
<br>&#xA0; &#xA0; echo </span><span class="default">$value </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>Output:
<br>1 2 3 4 5 6 7 8 9 
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">,</span><span class="default">5</span><span class="keyword">,</span><span class="default">6</span><span class="keyword">,</span><span class="default">7</span><span class="keyword">,</span><span class="default">8</span><span class="keyword">,</span><span class="default">9</span><span class="keyword">);
<br>
<br>while (list(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">) = </span><span class="default">each</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">))
<br>{
<br>&#xA0; &#xA0; unset(</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$key </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">]);
<br>&#xA0; &#xA0; echo </span><span class="default">$value </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>Output:
<br>1 3 5 7 9
<br>
<br>
<br>[EDIT BY danbrown AT php DOT net: Contains a typofix by (scissor AT phplabs DOT pl) on 30-JAN-2009.]</span>
</div>
  

#


<div class="phpcode"><span class="html">
WARNING: Looping through &quot;values by reference&quot; for &quot;extra performance&quot; is an old myth. It&apos;s actually WORSE!<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">one</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">) {<br>&#xA0; &#xA0; foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$val</span><span class="keyword">) { </span><span class="comment">// Normal Variable<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">$val</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>function </span><span class="default">two</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">) {<br>&#xA0; &#xA0; foreach(</span><span class="default">$arr </span><span class="keyword">as &amp;</span><span class="default">$val</span><span class="keyword">) { </span><span class="comment">// Reference To Value<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">echo </span><span class="default">$val</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">$a </span><span class="keyword">= array( </span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">);<br></span><span class="default">one</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br></span><span class="default">two</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Which do you think is faster?<br><br>Lots of people think the answer is two() because it uses &quot;reference to value, which it doesn&apos;t have to copy each value when it loops&quot;.<br><br>Well, that&apos;s totally wrong!<br><br>Here&apos;s what actually happens:<br><br>* one():<br><br>- This function takes an array as argument ($arr).<br>- The array function argument itself isn&apos;t passed by reference, so the function knows it isn&apos;t allowed to modify the original at all.<br>- Then the foreach loop happens. The array itself wasn&apos;t passed by reference to the function, so PHP knows that it isn&apos;t allowed to modify the outside array, so it therefore makes a copy of the array&apos;s internal iteration offset state (that&apos;s just a simple number which says which item you are currently at during things like foreach()), which costs almost no performance or memory at all since it&apos;s just a small number.<br>- Next, it uses that copied iteration offset to loop through all key/value pairs of the array (ie 0th key, 1st key, 2nd key, etc...). And the value at the current offset (a PHP &quot;zval&quot;) is assigned to a variable called $val.<br>- Does $val make a COPY of the value? That&apos;s what MANY people think. But the answer is NO. It DOESN&apos;T. It re-uses the existing value in memory. With zero performance cost. It&apos;s called &quot;copy-on-write&quot; and means that PHP doesn&apos;t make any copies unless you try to MODIFY the value.<br>- If you try to MODIFY $val, THEN it will allocate a NEW zval in memory and store $val there instead (but it still won&apos;t modify the original array, so you can rest assured).<br><br>Alright, so what&apos;s the second version doing? The beloved &quot;iterate values by reference&quot;?<br><br>* two():<br><br>- This function takes an array as argument ($arr).<br>- The array function argument itself isn&apos;t passed by reference, so the function knows it isn&apos;t allowed to modify the original at all.<br>- Then the foreach loop happens. The array itself wasn&apos;t passed by reference to the function, so PHP knows that it isn&apos;t allowed to modify the outside array.<br>- But it also sees that you want to look at all VALUES by reference (&amp;$val), so PHP says &quot;Uh oh, this is dangerous. If we just give them references to the original array&apos;s values, and they assign some new value to their reference, they would destroy the original array which they aren&apos;t allowed to touch!&quot;.<br>- So PHP makes a FULL COPY of the ENTIRE array and ALL VALUES before it starts iterating. YIKES!<br><br>Therefore: STOP using the old, mythological &quot;&amp;$val&quot; iteration method! It&apos;s almost always BAD! With worse performance, and risks of bugs and quirks as is demonstrated in the manual.<br><br>You can always manually write array assignments explicitly, without references, like this:<br><br><span class="default">&lt;?php<br><br>$a </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">);<br>foreach(</span><span class="default">$a </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">) {<br>&#xA0;&#xA0; </span><span class="default">$a</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$val </span><span class="keyword">* </span><span class="default">10</span><span class="keyword">;<br>}<br></span><span class="comment">// $a is now [10, 20, 30]<br><br></span><span class="default">?&gt;<br></span><br>The main lesson is this: DON&apos;T blindly iterate through values by reference! Telling PHP that you want direct references will force PHP to need to copy the WHOLE array to protect its original values! So instead, just loop normally and trust the fact that PHP *is* actually smart enough to never copy your original array&apos;s values! PHP uses &quot;copy-on-write&quot;, which means that attempting to assign something new to $val is the ONLY thing that causes a copying, and only of that SINGLE element! :-) But you never do that anyway, when iterating without reference. If you ever want to modify something, you use the &quot;$a[$key] = 123;&quot; method of updating the value.<br><br>Enjoy and good luck with your code! :-)</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to use the list for multidimension arrays, you can nest several lists:<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= [<br>&#xA0; &#xA0; [</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, array(</span><span class="default">3</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">)],<br>&#xA0; &#xA0; [</span><span class="default">3</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, array(</span><span class="default">5</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">)],<br>];<br><br>foreach (</span><span class="default">$array </span><span class="keyword">as list(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">, list(</span><span class="default">$c</span><span class="keyword">, </span><span class="default">$d</span><span class="keyword">))) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;A: </span><span class="default">$a</span><span class="string">; B: </span><span class="default">$b</span><span class="string">; C: </span><span class="default">$c</span><span class="string">; D: </span><span class="default">$d</span><span class="string">;&lt;br&gt;&quot;</span><span class="keyword">;<br>};<br></span><span class="default">?&gt;<br></span><br>Will output:<br>A: 1; B: 2; C: 3; D: 4;<br>A: 3; B: 4; C: 5; D: 6;<br><br>And:<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= [<br>&#xA0; &#xA0; [</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, array(</span><span class="default">3</span><span class="keyword">, array(</span><span class="default">4</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">))],<br>&#xA0; &#xA0; [</span><span class="default">3</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, array(</span><span class="default">5</span><span class="keyword">, array(</span><span class="default">6</span><span class="keyword">, </span><span class="default">7</span><span class="keyword">))],<br>];<br><br>foreach (</span><span class="default">$array </span><span class="keyword">as list(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">, list(</span><span class="default">$c</span><span class="keyword">, list(</span><span class="default">$d</span><span class="keyword">, </span><span class="default">$e</span><span class="keyword">)))) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;A: </span><span class="default">$a</span><span class="string">; B: </span><span class="default">$b</span><span class="string">; C: </span><span class="default">$c</span><span class="string">; D: </span><span class="default">$d</span><span class="string">; E: </span><span class="default">$e</span><span class="string">;&lt;br&gt;&quot;</span><span class="keyword">;<br>};<br></span><span class="default">Will output</span><span class="keyword">:<br></span><span class="default">A</span><span class="keyword">: </span><span class="default">1</span><span class="keyword">; </span><span class="default">B</span><span class="keyword">: </span><span class="default">2</span><span class="keyword">; </span><span class="default">C</span><span class="keyword">: </span><span class="default">3</span><span class="keyword">; </span><span class="default">D</span><span class="keyword">: </span><span class="default">4</span><span class="keyword">; </span><span class="default">E</span><span class="keyword">: </span><span class="default">5</span><span class="keyword">;<br></span><span class="default">A</span><span class="keyword">: </span><span class="default">3</span><span class="keyword">; </span><span class="default">B</span><span class="keyword">: </span><span class="default">4</span><span class="keyword">; </span><span class="default">C</span><span class="keyword">: </span><span class="default">5</span><span class="keyword">; </span><span class="default">D</span><span class="keyword">: </span><span class="default">6</span><span class="keyword">; </span><span class="default">E</span><span class="keyword">: </span><span class="default">7</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
in foreach if you want to iterate through a specific column in a nested arrays for example:<br><br>$arr = array(<br>&#xA0; &#xA0;&#xA0; [1, 2, 3,&#xA0;&#xA0; 4],<br>&#xA0; &#xA0;&#xA0; [14, 6, 7,&#xA0; 6],<br>&#xA0; &#xA0;&#xA0; [10, 2 ,3 , 2],<br>);<br><br>when we want to iterate on the third column we can use:<br><br>foreach( $arr as list( , , $a)) {<br>&#xA0; &#xA0; echo &quot;$a\n&quot;;<br>}<br><br>this will print:<br>3<br>7<br>3</span>
</div>
  

#


<div class="phpcode"><span class="html">
What happened to this note:<br>&quot;Unless the array is referenced, foreach operates on a copy of the specified array and not the array itself. foreach has some side effects on the array pointer. Don&apos;t rely on the array pointer during or after the foreach without resetting it.&quot;<br><br>Is this no longer the case?<br>It seems only to remain in the Serbian documentation: <a href="http://php.net/manual/sr/control-structures.foreach.php" rel="nofollow" target="_blank">http://php.net/manual/sr/control-structures.foreach.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
For those who&apos;d like to traverse an array including just added elements (within this very foreach), here&apos;s a workaround:
<br>
<br><span class="default">&lt;?php
<br>$values </span><span class="keyword">= array(</span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="default">2 </span><span class="keyword">=&gt; </span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="default">3 </span><span class="keyword">=&gt; </span><span class="string">&apos;c&apos;</span><span class="keyword">);
<br>while (list(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">) = </span><span class="default">each</span><span class="keyword">(</span><span class="default">$values</span><span class="keyword">)) {
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$key</span><span class="string"> =&gt; </span><span class="default">$value</span><span class="string"> \r\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; if (</span><span class="default">$key </span><span class="keyword">== </span><span class="default">3</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$values</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">] = </span><span class="string">&apos;d&apos;</span><span class="keyword">; 
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if (</span><span class="default">$key </span><span class="keyword">== </span><span class="default">4</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$values</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">] = </span><span class="string">&apos;e&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>the code above will output:
<br>
<br>1 =&gt; a
<br>2 =&gt; b
<br>3 =&gt; c
<br>4 =&gt; d
<br>5 =&gt; e</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.foreach.php)

**[To root](/README.md)**