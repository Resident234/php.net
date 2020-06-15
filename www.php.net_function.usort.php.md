# usort




<div class="phpcode"><span class="html">
Just wanted to show off the beauty of PHPs spaceship operator in this use case.<br><br><span class="default">&lt;?php </span><span class="comment">// tested on PHP 7.1<br></span><span class="default">$a </span><span class="keyword">= [</span><span class="default">2</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">, </span><span class="default">7</span><span class="keyword">];<br></span><span class="default">$asc </span><span class="keyword">= </span><span class="default">$desc </span><span class="keyword">= </span><span class="default">$a</span><span class="keyword">;<br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$asc</span><span class="keyword">, function (</span><span class="default">int $a</span><span class="keyword">, </span><span class="default">int $b</span><span class="keyword">) { return (</span><span class="default">$a </span><span class="keyword">&lt;=&gt; </span><span class="default">$b</span><span class="keyword">); });<br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$desc</span><span class="keyword">, function (</span><span class="default">int $a</span><span class="keyword">, </span><span class="default">int $b</span><span class="keyword">) { return -(</span><span class="default">$a </span><span class="keyword">&lt;=&gt; </span><span class="default">$b</span><span class="keyword">); });<br></span><span class="default">print_r</span><span class="keyword">([ </span><span class="default">$a</span><span class="keyword">, </span><span class="default">$asc</span><span class="keyword">, </span><span class="default">$desc </span><span class="keyword">]);<br><br></span><span class="comment">/**<br> * Getting ahead of myself but... If arrow function syntax was possible:<br> * usort($asc, (int $a, int $b) =&gt; ($a &lt;=&gt; $b));<br> * usort($desc, (int $a, int $b) =&gt; -($a &lt;=&gt; $b));<br> */<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When trying to do some custom sorting with objects and an anonymous function it wasn&apos;t entirely clear how this usort function works. I think it probably uses a quicksort in the background. Basically it actually moves the $b variable up or down in respect to the $a variable. It does NOT move the $a variable inside the callback function. This is key to getting your logic right in the comparisons.<br><br>If you return -1 that moves the $b variable down the array, return 1 moves $b up the array and return 0 keeps $b in the same place. <br><br>To test I cut down my code to sorting a simple array from highest priority to lowest.<br><br><span class="default">&lt;?php<br>$priorities </span><span class="keyword">= array(</span><span class="default">5</span><span class="keyword">, </span><span class="default">8</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">7</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">);<br><br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$priorities</span><span class="keyword">, function(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if (</span><span class="default">$a </span><span class="keyword">== </span><span class="default">$b</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;a (</span><span class="default">$a</span><span class="string">) is same priority as b (</span><span class="default">$b</span><span class="string">), keeping the same\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else if (</span><span class="default">$a </span><span class="keyword">&gt; </span><span class="default">$b</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;a (</span><span class="default">$a</span><span class="string">) is higher priority than b (</span><span class="default">$b</span><span class="string">), moving b down array\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return -</span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;b (</span><span class="default">$b</span><span class="string">) is higher priority than a (</span><span class="default">$a</span><span class="string">), moving b up array\n&quot;</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>});<br><br>echo </span><span class="string">&quot;Sorted priorities:\n&quot;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$priorities</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Output:<br><br>b (8) is higher priority than a (3), moving b up array<br>b (5) is higher priority than a (3), moving b up array<br>b (7) is higher priority than a (3), moving b up array<br>a (3) is same priority as b (3), keeping the same<br>a (8) is higher priority than b (3), moving b down array<br>b (8) is higher priority than a (7), moving b up array<br>b (8) is higher priority than a (5), moving b up array<br>b (8) is higher priority than a (3), moving b up array<br>a (5) is higher priority than b (3), moving b down array<br>a (7) is higher priority than b (5), moving b down array<br><br>Sorted priorities:<br>array(5) {<br>&#xA0; [0]=&gt; int(8)<br>&#xA0; [1]=&gt; int(7)<br>&#xA0; [2]=&gt; int(5)<br>&#xA0; [3]=&gt; int(3)<br>&#xA0; [4]=&gt; int(3)<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
this is a new multisort function for sorting on multiple subfield like it will be in sql : &apos;ORDER BY field1, field2&apos;
<br>number of sort field is undefined
<br><span class="default">&lt;?php
<br>
<br>$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">5</span><span class="keyword">);
<br></span><span class="default">$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">2</span><span class="keyword">);
<br></span><span class="default">$array</span><span class="keyword">[] = array(</span><span class="string">&apos;soc&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">=&gt;</span><span class="default">2</span><span class="keyword">);
<br>
<br></span><span class="comment">//usage
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">multiSort</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="string">&apos;soc&apos;</span><span class="keyword">, </span><span class="string">&apos;code&apos;</span><span class="keyword">));
<br>
<br>function </span><span class="default">multiSort</span><span class="keyword">() {
<br>&#xA0; &#xA0; </span><span class="comment">//get args of the function
<br>&#xA0; &#xA0; </span><span class="default">$args </span><span class="keyword">= </span><span class="default">func_get_args</span><span class="keyword">();
<br>&#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">);
<br>&#xA0; &#xA0; if (</span><span class="default">$c </span><span class="keyword">&lt; </span><span class="default">2</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">//get the array to sort
<br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">array_splice</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">$array</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];
<br>&#xA0; &#xA0; </span><span class="comment">//sort with an anoymous function using args
<br>&#xA0; &#xA0; </span><span class="default">usort</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, function(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) use(</span><span class="default">$args</span><span class="keyword">) {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$args</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cmp </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$cmp </span><span class="keyword">== </span><span class="default">0 </span><span class="keyword">&amp;&amp; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$c</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cmp </span><span class="keyword">= </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">[ </span><span class="default">$args</span><span class="keyword">[ </span><span class="default">$i </span><span class="keyword">] ], </span><span class="default">$b</span><span class="keyword">[ </span><span class="default">$args</span><span class="keyword">[ </span><span class="default">$i </span><span class="keyword">] ]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$cmp</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; });
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$array</span><span class="keyword">;
<br>
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>output:
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [1] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [2] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 2
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [3] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 2
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [4] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 2
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 5
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [5] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 3
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>&#xA0; &#xA0; [6] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [soc] =&gt; 3
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [code] =&gt; 2
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>
<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
I wrote a wrapper for usort that lets you use something similar to an SQL ORDER BY clause. It can sort arrays of associative arrays and arrays of objects and I think it would work with some hybrid case.
<br>
<br>Example of how the function works:
<br>
<br><span class="default">&lt;?php
<br>$testAry </span><span class="keyword">= array(
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">)
<br>);
<br>
<br></span><span class="default">Utility</span><span class="keyword">::</span><span class="default">orderBy</span><span class="keyword">(</span><span class="default">$testAry</span><span class="keyword">, </span><span class="string">&apos;a ASC, b DESC&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">//Result:
<br></span><span class="default">$testAry </span><span class="keyword">= array(
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">)
<br>);
<br></span><span class="default">?&gt;
<br></span>
<br>To sort an array of objects you would do something like:
<br>Utility::orderBy($objectAry, &apos;getCreationDate() DESC, getSubOrder() ASC&apos;);
<br>
<br>This would sort an array of objects that have methods getCreationDate() and getSubOrder().
<br>
<br>Here is the function:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">Utility </span><span class="keyword">{
<br>&#xA0; &#xA0; </span><span class="comment">/*
<br>&#xA0; &#xA0; * @param array $ary the array we want to sort
<br>&#xA0; &#xA0; * @param string $clause a string specifying how to sort the array similar to SQL ORDER BY clause
<br>&#xA0; &#xA0; * @param bool $ascending that default sorts fall back to when no direction is specified
<br>&#xA0; &#xA0; * @return null
<br>&#xA0; &#xA0; */
<br>&#xA0; &#xA0; </span><span class="keyword">public static function </span><span class="default">orderBy</span><span class="keyword">(&amp;</span><span class="default">$ary</span><span class="keyword">, </span><span class="default">$clause</span><span class="keyword">, </span><span class="default">$ascending </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$clause </span><span class="keyword">= </span><span class="default">str_ireplace</span><span class="keyword">(</span><span class="string">&apos;order by&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$clause</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$clause </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\s+/&apos;</span><span class="keyword">, </span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">$clause</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$clause</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dirMap </span><span class="keyword">= array(</span><span class="string">&apos;desc&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;asc&apos; </span><span class="keyword">=&gt; -</span><span class="default">1</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$def </span><span class="keyword">= </span><span class="default">$ascending </span><span class="keyword">? -</span><span class="default">1 </span><span class="keyword">: </span><span class="default">1</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keyAry </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dirAry </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$keys </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$key </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keyAry</span><span class="keyword">[] = </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(isset(</span><span class="default">$key</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dir </span><span class="keyword">= </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dirAry</span><span class="keyword">[] = </span><span class="default">$dirMap</span><span class="keyword">[</span><span class="default">$dir</span><span class="keyword">] ? </span><span class="default">$dirMap</span><span class="keyword">[</span><span class="default">$dir</span><span class="keyword">] : </span><span class="default">$def</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dirAry</span><span class="keyword">[] = </span><span class="default">$def</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fnBody </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; for(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$keyAry</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">--) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$k </span><span class="keyword">= </span><span class="default">$keyAry</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$t </span><span class="keyword">= </span><span class="default">$dirAry</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$f </span><span class="keyword">= -</span><span class="default">1 </span><span class="keyword">* </span><span class="default">$t</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$aStr </span><span class="keyword">= </span><span class="string">&apos;$a[\&apos;&apos;</span><span class="keyword">.</span><span class="default">$k</span><span class="keyword">.</span><span class="string">&apos;\&apos;]&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bStr </span><span class="keyword">= </span><span class="string">&apos;$b[\&apos;&apos;</span><span class="keyword">.</span><span class="default">$k</span><span class="keyword">.</span><span class="string">&apos;\&apos;]&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$k</span><span class="keyword">, </span><span class="string">&apos;(&apos;</span><span class="keyword">) !== </span><span class="default">false</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$aStr </span><span class="keyword">= </span><span class="string">&apos;$a-&gt;&apos;</span><span class="keyword">.</span><span class="default">$k</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bStr </span><span class="keyword">= </span><span class="string">&apos;$b-&gt;&apos;</span><span class="keyword">.</span><span class="default">$k</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$fnBody </span><span class="keyword">== </span><span class="string">&apos;&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fnBody </span><span class="keyword">.= </span><span class="string">&quot;if(</span><span class="keyword">{</span><span class="default">$aStr</span><span class="keyword">}</span><span class="string"> == </span><span class="keyword">{</span><span class="default">$bStr</span><span class="keyword">}</span><span class="string">) { return 0; }\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fnBody </span><span class="keyword">.= </span><span class="string">&quot;return (</span><span class="keyword">{</span><span class="default">$aStr</span><span class="keyword">}</span><span class="string"> &lt; </span><span class="keyword">{</span><span class="default">$bStr</span><span class="keyword">}</span><span class="string">) ? </span><span class="keyword">{</span><span class="default">$t</span><span class="keyword">}</span><span class="string"> : </span><span class="keyword">{</span><span class="default">$f</span><span class="keyword">}</span><span class="string">;\n&quot;</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fnBody </span><span class="keyword">= </span><span class="string">&quot;if(</span><span class="keyword">{</span><span class="default">$aStr</span><span class="keyword">}</span><span class="string"> == </span><span class="keyword">{</span><span class="default">$bStr</span><span class="keyword">}</span><span class="string">) {\n&quot; </span><span class="keyword">. </span><span class="default">$fnBody</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fnBody </span><span class="keyword">.= </span><span class="string">&quot;}\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fnBody </span><span class="keyword">.= </span><span class="string">&quot;return (</span><span class="keyword">{</span><span class="default">$aStr</span><span class="keyword">}</span><span class="string"> &lt; </span><span class="keyword">{</span><span class="default">$bStr</span><span class="keyword">}</span><span class="string">) ? </span><span class="keyword">{</span><span class="default">$t</span><span class="keyword">}</span><span class="string"> : </span><span class="keyword">{</span><span class="default">$f</span><span class="keyword">}</span><span class="string">;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$fnBody</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$sortFn </span><span class="keyword">= </span><span class="default">create_function</span><span class="keyword">(</span><span class="string">&apos;$a,$b&apos;</span><span class="keyword">, </span><span class="default">$fnBody</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">usort</span><span class="keyword">(</span><span class="default">$ary</span><span class="keyword">, </span><span class="default">$sortFn</span><span class="keyword">);&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can also sort multi-dimensional array for multiple values like as<br><br><span class="default">&lt;?php <br>$arr </span><span class="keyword">= [<br>&#xA0; &#xA0; [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;Sally&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;nick_name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;sal&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;availability&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;0&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;is_fav&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;0&quot;<br>&#xA0; &#xA0; </span><span class="keyword">],<br>&#xA0; &#xA0; [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;David&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;nick_name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;dav07&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;availability&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;0&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;is_fav&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;1&quot;<br>&#xA0; &#xA0; </span><span class="keyword">],<br>&#xA0; &#xA0; [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;Zen&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;nick_name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;zen&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;availability&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;1&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;is_fav&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;0&quot;<br>&#xA0; &#xA0; </span><span class="keyword">],<br>&#xA0; &#xA0; [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;Jackson&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;nick_name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;jack&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;availability&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;1&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;is_fav&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;1&quot;<br>&#xA0; &#xA0; </span><span class="keyword">],<br>&#xA0; &#xA0; [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;Rohit&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;nick_name&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;rod&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;availability&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;0&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;is_fav&quot;</span><span class="keyword">=&gt; </span><span class="string">&quot;0&quot;<br>&#xA0; &#xA0; </span><span class="keyword">],<br><br>];<br><br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">,function(</span><span class="default">$a</span><span class="keyword">,</span><span class="default">$b</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">= </span><span class="default">$b</span><span class="keyword">[</span><span class="string">&apos;is_fav&apos;</span><span class="keyword">] - </span><span class="default">$a</span><span class="keyword">[</span><span class="string">&apos;is_fav&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">.= </span><span class="default">$b</span><span class="keyword">[</span><span class="string">&apos;availability&apos;</span><span class="keyword">] - </span><span class="default">$a</span><span class="keyword">[</span><span class="string">&apos;availability&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; </span><span class="default">$c </span><span class="keyword">.= </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">[</span><span class="string">&apos;nick_name&apos;</span><span class="keyword">],</span><span class="default">$b</span><span class="keyword">[</span><span class="string">&apos;nick_name&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; return </span><span class="default">$c</span><span class="keyword">;<br>});<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Output:<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Jackson<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [nick_name] =&gt; jack<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [availability] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [is_fav] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; David<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [nick_name] =&gt; dav07<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [availability] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [is_fav] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [2] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Zen<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [nick_name] =&gt; zen<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [availability] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [is_fav] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [3] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Rohit<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [nick_name] =&gt; rod<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [availability] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [is_fav] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [4] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Sally<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [nick_name] =&gt; sal<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [availability] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [is_fav] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can also sort multi-dimensional array for multiple values like as<br><span class="default">&lt;?php<br>$arr </span><span class="keyword">= array(<br>&#xA0; &#xA0; array(</span><span class="string">&apos;id&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;age&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;sex&apos;</span><span class="keyword">=&gt;</span><span class="default">6</span><span class="keyword">, </span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;a&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; array(</span><span class="string">&apos;id&apos;</span><span class="keyword">=&gt;</span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;age&apos;</span><span class="keyword">=&gt;</span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;sex&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;c&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; array(</span><span class="string">&apos;id&apos;</span><span class="keyword">=&gt;</span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;age&apos;</span><span class="keyword">=&gt;</span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;sex&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;b&apos;</span><span class="keyword">),<br>&#xA0; &#xA0; array(</span><span class="string">&apos;id&apos;</span><span class="keyword">=&gt;</span><span class="default">4</span><span class="keyword">, </span><span class="string">&apos;age&apos;</span><span class="keyword">=&gt;</span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;sex&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;d&apos;</span><span class="keyword">),<br>);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">arrayOrderBy</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="string">&apos;age asc,sex asc,name desc&apos;</span><span class="keyword">));<br><br>function </span><span class="default">arrayOrderBy</span><span class="keyword">(array &amp;</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">$order </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">) {<br>&#xA0; &#xA0; if (</span><span class="default">is_null</span><span class="keyword">(</span><span class="default">$order</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$arr</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">$orders </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$order</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">usort</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, function(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) use(</span><span class="default">$orders</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$orders </span><span class="keyword">as </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$field</span><span class="keyword">, </span><span class="default">$sort</span><span class="keyword">) = </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;trim&apos;</span><span class="keyword">, </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">)));<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!(isset(</span><span class="default">$a</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">]) &amp;&amp; isset(</span><span class="default">$b</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">]))) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">strcasecmp</span><span class="keyword">(</span><span class="default">$sort</span><span class="keyword">, </span><span class="string">&apos;desc&apos;</span><span class="keyword">) === </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">$a</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$a </span><span class="keyword">= </span><span class="default">$b</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$b </span><span class="keyword">= </span><span class="default">$tmp</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">]) &amp;&amp; </span><span class="default">is_numeric</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">]) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[] = </span><span class="default">$a</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">] - </span><span class="default">$b</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[] = </span><span class="default">strcmp</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">], </span><span class="default">$b</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$result</span><span class="keyword">);<br>&#xA0; &#xA0; });<br>&#xA0; &#xA0; return </span><span class="default">$arr</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>output:<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [age] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [sex] =&gt; 6<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; a<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [age] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [sex] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; d<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [2] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [age] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [sex] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; c<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [3] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [age] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [sex] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; b<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
As the manual says, &quot;If two members compare as equal, their order in the sorted array is undefined.&quot; This means that the sort used is not &quot;stable&quot; and may change the order of elements that compare equal.
<br>
<br>Sometimes you really do need a stable sort. For example, if you sort a list by one field, then sort it again by another field, but don&apos;t want to lose the ordering from the previous field. In that case it is better to use usort with a comparison function that takes both fields into account, but if you can&apos;t do that then use the function below. It is a merge sort, which is guaranteed O(n*log(n)) complexity, which means it stays reasonably fast even when you use larger lists (unlike bubblesort and insertion sort, which are O(n^2)). 
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mergesort</span><span class="keyword">(&amp;</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$cmp_function </span><span class="keyword">= </span><span class="string">&apos;strcmp&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">// Arrays of size &lt; 2 require no action.
<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) &lt; </span><span class="default">2</span><span class="keyword">) return;
<br>&#xA0; &#xA0; </span><span class="comment">// Split the array in half
<br>&#xA0; &#xA0; </span><span class="default">$halfway </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) / </span><span class="default">2</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$array1 </span><span class="keyword">= </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$halfway</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$array2 </span><span class="keyword">= </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$halfway</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="comment">// Recurse to sort the two halves
<br>&#xA0; &#xA0; </span><span class="default">mergesort</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">, </span><span class="default">$cmp_function</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">mergesort</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">, </span><span class="default">$cmp_function</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="comment">// If all of $array1 is &lt;= all of $array2, just append them.
<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$cmp_function</span><span class="keyword">, </span><span class="default">end</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">), </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) &lt; </span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; return;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">// Merge the two sorted arrays into a single sorted array
<br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$ptr1 </span><span class="keyword">= </span><span class="default">$ptr2 </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; while (</span><span class="default">$ptr1 </span><span class="keyword">&lt; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">) &amp;&amp; </span><span class="default">$ptr2 </span><span class="keyword">&lt; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$cmp_function</span><span class="keyword">, </span><span class="default">$array1</span><span class="keyword">[</span><span class="default">$ptr1</span><span class="keyword">], </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$ptr2</span><span class="keyword">]) &lt; </span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">$array1</span><span class="keyword">[</span><span class="default">$ptr1</span><span class="keyword">++];
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$ptr2</span><span class="keyword">++];
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="comment">// Merge the remainder
<br>&#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$ptr1 </span><span class="keyword">&lt; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">)) </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">$array1</span><span class="keyword">[</span><span class="default">$ptr1</span><span class="keyword">++];
<br>&#xA0; &#xA0; while (</span><span class="default">$ptr2 </span><span class="keyword">&lt; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">)) </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$ptr2</span><span class="keyword">++];
<br>&#xA0; &#xA0; return;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to sort an array according to another array acting as a priority list, you can use this function.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">listcmp</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)
<br>{
<br>&#xA0; global </span><span class="default">$order</span><span class="keyword">;
<br>
<br>&#xA0; foreach(</span><span class="default">$order </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; if(</span><span class="default">$a</span><span class="keyword">==</span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; if(</span><span class="default">$b</span><span class="keyword">==</span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$order</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="string">&quot;first&quot;</span><span class="keyword">;
<br></span><span class="default">$order</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="string">&quot;second&quot;</span><span class="keyword">;
<br></span><span class="default">$order</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = </span><span class="string">&quot;third&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="string">&quot;second&quot;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="string">&quot;first&quot;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = </span><span class="string">&quot;third&quot;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] = </span><span class="string">&quot;fourth&quot;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">] = </span><span class="string">&quot;second&quot;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">] = </span><span class="string">&quot;first&quot;</span><span class="keyword">;
<br></span><span class="default">$array</span><span class="keyword">[</span><span class="default">6</span><span class="keyword">] = </span><span class="string">&quot;second&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">usort</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="string">&quot;listcmp&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.usort.php)

**[To root](/README.md)**