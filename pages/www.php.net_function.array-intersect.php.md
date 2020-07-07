# array_intersect




<div class="phpcode"><span class="html">
A clearer example of the key preservation of this function:<br><br><span class="default">&lt;?php<br><br>$array1 </span><span class="keyword">= array(</span><span class="default">2</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">, </span><span class="default">8</span><span class="keyword">, </span><span class="default">10</span><span class="keyword">, </span><span class="default">12</span><span class="keyword">);<br></span><span class="default">$array2 </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_intersect</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">));<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_intersect</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">, </span><span class="default">$array1</span><span class="keyword">));<br><br></span><span class="default">?&gt;<br></span><br>yields the following:<br><br>array(3) {<br>&#xA0; [0]=&gt; int(2)<br>&#xA0; [1]=&gt; int(4)<br>&#xA0; [2]=&gt; int(6)<br>}<br><br>array(3) {<br>&#xA0; [1]=&gt; int(2)<br>&#xA0; [3]=&gt; int(4)<br>&#xA0; [5]=&gt; int(6)<br>}<br><br>This makes it important to remember which way round you passed the arrays to the function if these keys are relied on later in the script.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a array_union($a, $b):<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//&#xA0; $a = 1 2 3 4<br>&#xA0; &#xA0; </span><span class="default">$union </span><span class="keyword">=&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//&#xA0; $b =&#xA0;&#xA0; 2&#xA0;&#xA0; 4 5 6<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_merge</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_intersect</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">),&#xA0; &#xA0; </span><span class="comment">//&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 2&#xA0;&#xA0; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">),&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">//&#xA0; &#xA0; &#xA0;&#xA0; 1&#xA0;&#xA0; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_diff</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 5 6<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//&#xA0; $u = 1 2 3 4 5 6<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need to supply arbitrary number of arguments <br>to array_intersect() or other array function, <br>use following function:<br><br>$full=call_user_func_array(&apos;array_intersect&apos;, $any_number_of_arrays_here);</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that array_intersect and array_unique doesnt work well with multidimensional arrays.<br>If you have, for example, <br><br><span class="default">&lt;?php<br><br>$orders_today</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = array(</span><span class="string">&apos;John Doe&apos;</span><span class="keyword">, </span><span class="string">&apos;PHP Book&apos;</span><span class="keyword">);<br></span><span class="default">$orders_today</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = array(</span><span class="string">&apos;Jack Smith&apos;</span><span class="keyword">, </span><span class="string">&apos;Coke&apos;</span><span class="keyword">);<br><br></span><span class="default">$orders_yesterday</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = array(</span><span class="string">&apos;Miranda Jones&apos;</span><span class="keyword">, </span><span class="string">&apos;Digital Watch&apos;</span><span class="keyword">);<br></span><span class="default">$orders_yesterday</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = array(</span><span class="string">&apos;John Doe&apos;</span><span class="keyword">, </span><span class="string">&apos;PHP Book&apos;</span><span class="keyword">);<br></span><span class="default">$orders_yesterday</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = array(</span><span class="string">&apos;Z&#xFFFD; da Silva&apos;</span><span class="keyword">, </span><span class="string">&apos;BMW Car&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>and wants to know if the same person bought the same thing today and yesterday and use array_intersect($orders_today, $orders_yesterday) you&apos;ll get as result:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">Array<br>(<br>&#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="default">John Doe<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] =&gt; </span><span class="default">PHP Book<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br><br>&#xA0; &#xA0; [</span><span class="default">1</span><span class="keyword">] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="default">Jack Smith<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] =&gt; </span><span class="default">Coke<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br><br>)<br><br></span><span class="default">?&gt;<br></span><br>but we can get around that by serializing the inner arrays:<br><span class="default">&lt;?php<br><br>$orders_today</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="default">serialize</span><span class="keyword">(array(</span><span class="string">&apos;John Doe&apos;</span><span class="keyword">, </span><span class="string">&apos;PHP Book&apos;</span><span class="keyword">));<br></span><span class="default">$orders_today</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="default">serialize</span><span class="keyword">(array(</span><span class="string">&apos;Jack Smith&apos;</span><span class="keyword">, </span><span class="string">&apos;Coke&apos;</span><span class="keyword">));<br><br></span><span class="default">$orders_yesterday</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="default">serialize</span><span class="keyword">(array(</span><span class="string">&apos;Miranda Jones&apos;</span><span class="keyword">, </span><span class="string">&apos;Digital Watch&apos;</span><span class="keyword">));<br></span><span class="default">$orders_yesterday</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="default">serialize</span><span class="keyword">(array(</span><span class="string">&apos;John Doe&apos;</span><span class="keyword">, </span><span class="string">&apos;PHP Book&apos;</span><span class="keyword">));<br></span><span class="default">$orders_yesterday</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = </span><span class="default">serialize</span><span class="keyword">(array(</span><span class="string">&apos;Z&#xFFFD; da Silva&apos;</span><span class="keyword">, </span><span class="string">&apos;Uncle Tungsten&apos;</span><span class="keyword">));<br><br></span><span class="default">?&gt;<br></span><br>so that array_map(&quot;unserialize&quot;, array_intersect($orders_today, $orders_yesterday)) will return:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">Array<br>(<br>&#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">0</span><span class="keyword">] =&gt; </span><span class="default">John Doe<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] =&gt; </span><span class="default">PHP Book<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br><br>)<br><br></span><span class="default">?&gt;<br></span><br>showing us who bought the same thing today and yesterday =)<br><br>[]s</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-intersect.php)

**[To root](/README.md)**