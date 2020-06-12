# array_fill




<div class="phpcode"><span class="html">
This is what I recently did to quickly create a two dimensional array (10x10), initialized to 0:<br><br><span class="default">&lt;?php<br>&#xA0; $a </span><span class="keyword">= </span><span class="default">array_fill</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">10</span><span class="keyword">, </span><span class="default">array_fill</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">10</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>This should work for as many dimensions as you want, each time passing to array_fill() (as the 3rd argument) another array_fill() function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need negative indices:<br><span class="default">&lt;?php<br>$b </span><span class="keyword">= </span><span class="default">array_fill</span><span class="keyword">(-</span><span class="default">2</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, </span><span class="string">&apos;pear&apos;</span><span class="keyword">);</span><span class="comment">//this is not what we want<br></span><span class="default">$c </span><span class="keyword">= </span><span class="default">array_fill_keys</span><span class="keyword">(</span><span class="default">range</span><span class="keyword">(-</span><span class="default">2</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">),</span><span class="string">&apos;pear&apos;</span><span class="keyword">);</span><span class="comment">//these are negative indices<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>Here is result of the code above:<br>Array<br>(<br>&#xA0; &#xA0; [-2] =&gt; pear<br>&#xA0; &#xA0; [0] =&gt; pear<br>&#xA0; &#xA0; [1] =&gt; pear<br>&#xA0; &#xA0; [2] =&gt; pear<br>)<br>Array<br>(<br>&#xA0; &#xA0; [-2] =&gt; pear<br>&#xA0; &#xA0; [-1] =&gt; pear<br>&#xA0; &#xA0; [0] =&gt; pear<br>&#xA0; &#xA0; [1] =&gt; pear<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using objects with array_fill may cause unexpected results. Consider the following:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Foo </span><span class="keyword">{<br>&#xA0;&#xA0; public </span><span class="default">$bar </span><span class="keyword">= </span><span class="string">&quot;banana&quot;</span><span class="keyword">;<br>}<br><br></span><span class="comment">//fill an array with objects<br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">array_fill</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, new </span><span class="default">Foo</span><span class="keyword">());<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br></span><span class="comment">/*<br>array(2) {<br>&#xA0; [0]=&gt;<br>&#xA0; object(Foo)#1 (1) {<br>&#xA0; &#xA0; [&quot;bar&quot;]=&gt;<br>&#xA0; &#xA0; string(6) &quot;banana&quot;<br>&#xA0; }<br>&#xA0; [1]=&gt;<br>&#xA0; object(Foo)#1 (1) {<br>&#xA0; &#xA0; [&quot;bar&quot;]=&gt;<br>&#xA0; &#xA0; string(6) &quot;banana&quot;<br>&#xA0; }<br>} */<br><br>//now we change the attribute of the object stored in index 0<br>//this actually changes the attribute for EACH object in the ENTIRE array<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]-&gt;</span><span class="default">bar </span><span class="keyword">= </span><span class="string">&quot;apple&quot;</span><span class="keyword">;<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br></span><span class="comment">/*<br>array(2) {<br>&#xA0; [0]=&gt;<br>&#xA0; object(Foo)#1 (1) {<br>&#xA0; &#xA0; [&quot;bar&quot;]=&gt;<br>&#xA0; &#xA0; string(5) &quot;apple&quot;<br>&#xA0; }<br>&#xA0; [1]=&gt;<br>&#xA0; object(Foo)#1 (1) {<br>&#xA0; &#xA0; [&quot;bar&quot;]=&gt;<br>&#xA0; &#xA0; string(5) &quot;apple&quot;<br>&#xA0; }<br>}<br> */<br></span><span class="default">?&gt;<br></span><br>Objects are filled in the array BY REFERENCE. They are not copied for each element in the array.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-fill.php)

**[⬆ to root](/)**