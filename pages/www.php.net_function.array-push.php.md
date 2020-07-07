# array_push




<div class="phpcode"><span class="html">
If you&apos;re going to use array_push() to insert a &quot;$key&quot; =&gt; &quot;$value&quot; pair into an array, it can be done using the following:<br><br>&#xA0; &#xA0; $data[$key] = $value;<br><br>It is not necessary to use array_push.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;ve done a small comparison between array_push() and the $array[] method and the $array[] seems to be a lot faster.<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= array();<br>for (</span><span class="default">$x </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$x </span><span class="keyword">&lt;= </span><span class="default">100000</span><span class="keyword">; </span><span class="default">$x</span><span class="keyword">++)<br>{<br>&#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">$x</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span>takes 0.0622200965881 seconds<br><br>and<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= array();<br>for (</span><span class="default">$x </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$x </span><span class="keyword">&lt;= </span><span class="default">100000</span><span class="keyword">; </span><span class="default">$x</span><span class="keyword">++)<br>{<br>&#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$x</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span>takes 1.63195490837 seconds<br><br>so if your not making use of the return value of array_push() its better to use the $array[] way.<br><br>Hope this helps someone.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Rodrigo de Aquino asserted that instead of using array_push to append to an associative array you can instead just do...<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $data[$key] = $value;<br><br>...but this is actually not true. Unlike array_push and even...<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $data[] = $value;<br><br>...Rodrigo&apos;s suggestion is NOT guaranteed to append the new element to the END of the array. For instance...<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $data[&apos;one&apos;] = 1;<br>&#xA0; &#xA0; &#xA0; &#xA0; $data[&apos;two&apos;] = 2;<br>&#xA0; &#xA0; &#xA0; &#xA0; $data[&apos;three&apos;] = 3;<br>&#xA0; &#xA0; &#xA0; &#xA0; $data[&apos;four&apos;] = 4;<br> <br>...might very well result in an array that looks like this...<br><br>&#xA0; &#xA0; &#xA0;&#xA0; [ &quot;four&quot; =&gt; 4, &quot;one&quot; =&gt; 1, &quot;three&quot; =&gt; 3, &quot;two&quot; =&gt; 2 ]<br><br>I can only assume that PHP sorts the array as elements are added to make it easier for it to find a specified element by its key later. In many cases it won&apos;t matter if the array is not stored internally in the same order you added the elements, but if, for instance, you execute a foreach on the array later, the elements may not be processed in the order you need them to be.<br><br>If you want to add elements to the END of an associative array you should use the unary array union operator (+=) instead...<br><br>&#xA0; &#xA0; &#xA0;&#xA0; $data[&apos;one&apos;] = 1;<br>&#xA0; &#xA0; &#xA0;&#xA0; $data += [ &quot;two&quot; =&gt; 2 ];<br>&#xA0; &#xA0; &#xA0;&#xA0; $data += [ &quot;three&quot; =&gt; 3 ];<br>&#xA0; &#xA0; &#xA0;&#xA0; $data += [ &quot;four&quot; =&gt; 4 ];<br><br>You can also, of course, append more than one element at once...<br><br>&#xA0; &#xA0; &#xA0;&#xA0; $data[&apos;one&apos;] = 1;<br>&#xA0; &#xA0; &#xA0;&#xA0; $data += [ &quot;two&quot; =&gt; 2, &quot;three&quot; =&gt; 3 ];<br>&#xA0; &#xA0; &#xA0;&#xA0; $data += [ &quot;four&quot; =&gt; 4 ];<br><br>Note that like array_push (but unlike $array[] =) the array must exist before the unary union, which means that if you are building an array in a loop you need to declare an empty array first...<br><br>&#xA0; &#xA0; &#xA0;&#xA0; $data = [];<br>&#xA0; &#xA0; &#xA0;&#xA0; for ( $i = 1; $i &lt; 5; $i++ ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $data += [ &quot;element$i&quot; =&gt; $i ];<br>&#xA0; &#xA0; &#xA0;&#xA0; }<br><br>...which will result in an array that looks like this...<br><br>&#xA0; &#xA0; &#xA0; [ &quot;element1&quot; =&gt; 1, &quot;element2&quot; =&gt; 2, &quot;element3&quot; =&gt; 3, &quot;element4&quot; =&gt; 4 ]</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is a mistake in the note by egingell at sisna dot com 12 years ago. The tow dimensional array will output &quot;d,e,f&quot;, not &quot;a,b,c&quot;.<br><br><span class="default">&lt;?php<br>$stack </span><span class="keyword">= array(</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="string">&apos;c&apos;</span><span class="keyword">);<br></span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$stack</span><span class="keyword">, array(</span><span class="string">&apos;d&apos;</span><span class="keyword">, </span><span class="string">&apos;e&apos;</span><span class="keyword">, </span><span class="string">&apos;f&apos;</span><span class="keyword">));<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$stack</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The above will output this:<br>Array (<br>&#xA0; [0] =&gt; a<br>&#xA0; [1] =&gt; b<br>&#xA0; [2] =&gt; c<br>&#xA0; [3] =&gt; Array (<br>&#xA0; &#xA0;&#xA0; [0] =&gt; d<br>&#xA0; &#xA0;&#xA0; [1] =&gt; e<br>&#xA0; &#xA0;&#xA0; [2] =&gt; f<br>&#xA0; )<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re adding multiple values to an array in a loop, it&apos;s faster to use array_push than repeated [] = statements that I see all the time:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">timer<br></span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$start</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; private </span><span class="default">$end</span><span class="keyword">;<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">timer</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">start </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">Finish</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">end </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; private function </span><span class="default">GetStart</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">start</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">start</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; private function </span><span class="default">GetEnd</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">end</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">end</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">GetDiff</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">GetEnd</span><span class="keyword">() - </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">GetStart</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">Reset</span><span class="keyword">()<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">start </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>}<br><br>echo </span><span class="string">&quot;Adding 100k elements to array with []\n\n&quot;</span><span class="keyword">;<br></span><span class="default">$ta </span><span class="keyword">= array();<br></span><span class="default">$test </span><span class="keyword">= new </span><span class="default">Timer</span><span class="keyword">();<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">100000</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>}<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Finish</span><span class="keyword">();<br>echo </span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">GetDiff</span><span class="keyword">();<br><br>echo </span><span class="string">&quot;\n\nAdding 100k elements to array with array_push\n\n&quot;</span><span class="keyword">;<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Reset</span><span class="keyword">();<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">100000</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$ta</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">);<br>}<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Finish</span><span class="keyword">();<br>echo </span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">GetDiff</span><span class="keyword">();<br><br>echo </span><span class="string">&quot;\n\nAdding 100k elements to array with [] 10 per iteration\n\n&quot;</span><span class="keyword">;<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Reset</span><span class="keyword">();<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">10000</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ta</span><span class="keyword">[] = </span><span class="default">$i</span><span class="keyword">;<br>}<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Finish</span><span class="keyword">();<br>echo </span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">GetDiff</span><span class="keyword">();<br><br>echo </span><span class="string">&quot;\n\nAdding 100k elements to array with array_push 10 per iteration\n\n&quot;</span><span class="keyword">;<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Reset</span><span class="keyword">();<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">10000</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++)<br>{<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$ta</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">,</span><span class="default">$i</span><span class="keyword">);<br>}<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">Finish</span><span class="keyword">();<br>echo </span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">GetDiff</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>Output<br><br>$ php5 arraypush.php<br>X-Powered-By: PHP/5.2.5<br>Content-type: text/html<br><br>Adding 100k elements to array with []<br><br>0.044686794281006<br><br>Adding 100k elements to array with array_push<br><br>0.072616100311279<br><br>Adding 100k elements to array with [] 10 per iteration<br><br>0.034690141677856<br><br>Adding 100k elements to array with array_push 10 per iteration<br><br>0.023932933807373</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you push an array onto the stack, PHP will add the whole array to the next element instead of adding the keys and values to the array. If this is not what you want, you&apos;re better off using array_merge() or traverse the array you&apos;re pushing on and add each element with $stack[$key] = $value.<br><br><span class="default">&lt;?php<br><br>$stack </span><span class="keyword">= array(</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="string">&apos;c&apos;</span><span class="keyword">);<br></span><span class="default">array_push</span><span class="keyword">(</span><span class="default">$stack</span><span class="keyword">, array(</span><span class="string">&apos;d&apos;</span><span class="keyword">, </span><span class="string">&apos;e&apos;</span><span class="keyword">, </span><span class="string">&apos;f&apos;</span><span class="keyword">));<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$stack</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span>The above will output this:<br>Array (<br>&#xA0; [0] =&gt; a<br>&#xA0; [1] =&gt; b<br>&#xA0; [2] =&gt; c<br>&#xA0; [3] =&gt; Array (<br>&#xA0; &#xA0;&#xA0; [0] =&gt; a<br>&#xA0; &#xA0;&#xA0; [1] =&gt; b<br>&#xA0; &#xA0;&#xA0; [2] =&gt; c<br>&#xA0; )<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-push.php)

**[To root](/README.md)**