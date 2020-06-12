# Expressions




<div class="phpcode"><span class="html">
Manual defines &quot;expression is anything that has value&quot;, Therefore, parser will give error for following code.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) ? echo(</span><span class="string">&apos;true&apos;</span><span class="keyword">) : echo(</span><span class="string">&apos;false&apos;</span><span class="keyword">);
<br></span><span class="default">Note</span><span class="keyword">: </span><span class="string">&quot;? : &quot; </span><span class="default">operator has this syntax&#xA0; </span><span class="string">&quot;expr ? expr : expr;&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>since echo does not have(return) value and ?: expects expression(value).
<br>
<br>However, if function/language constructs that have/return value, such as include(), parser compiles code.
<br>
<br>Note: User defined functions always have/return value without explicit return statement (returns NULL if there is no return statement). Therefore, user defined functions are always valid expressions. 
<br>[It may be useful to have VOID as new type to prevent programmer to use function as RVALUE by mistake]
<br>
<br>For example,
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) ? include(</span><span class="string">&apos;true.inc&apos;</span><span class="keyword">) : include(</span><span class="string">&apos;false.inc&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>is valid, since &quot;include&quot; returns value.
<br>
<br>The fact &quot;echo&quot; does not return value(=&quot;echo&quot; is not a expression), is less obvious to me. 
<br>
<br>Print() and Echo() is NOT identical since print() has/returns value and can be a valid expression.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that even though PHP borrows large portions of its syntax from C, the &apos;,&apos; is treated quite differently. It&apos;s not possible to create combined expressions in PHP using the comma-operator that C has, except in for() loops.<br><br>Example (parse error):<br><br><span class="default">&lt;?php<br><br>$a </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">= </span><span class="default">4</span><span class="keyword">;<br><br>echo </span><span class="default">$a</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>echo </span><span class="default">$b</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>Example (works):<br><span class="default">&lt;?php<br><br></span><span class="keyword">for (</span><span class="default">$a </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">= </span><span class="default">4</span><span class="keyword">; </span><span class="default">$a </span><span class="keyword">&lt; </span><span class="default">3</span><span class="keyword">; </span><span class="default">$a</span><span class="keyword">++)<br>{<br>&#xA0; echo </span><span class="default">$a</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0; echo </span><span class="default">$b</span><span class="keyword">.</span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>This is because PHP doesn&apos;t actually have a proper comma-operator, it&apos;s only supported as syntactic sugar in for() loop headers. In C, it would have been perfectly legitimate to have this:<br><br>int f()<br>{<br>&#xA0; int a, b;<br>&#xA0; a = 2, b = 4;<br><br>&#xA0; return a;<br>}<br><br>or even this:<br><br>int g()<br>{<br>&#xA0; int a, b;<br>&#xA0; a = (2, b = 4);<br><br>&#xA0; return a;<br>}<br><br>In f(), a would have been set to 2, and b would have been set to 4.<br>In g(), (2, b = 4) would be a single expression which evaluates to 4, so both a and b would have been set to 4.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that there is a difference between a function and a function call, and both<br>are expressions. PHP has two kinds of function, &quot;named functions&quot; and &quot;anonymous<br>functions&quot;. Here&apos;s an example with both:<br><br><span class="default">&lt;?php<br></span><span class="comment">// A named function. Its name is &quot;double&quot;.<br></span><span class="keyword">function </span><span class="default">double</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">) {<br>&#xA0; return </span><span class="default">2 </span><span class="keyword">* </span><span class="default">$x</span><span class="keyword">;<br>}<br><br></span><span class="comment">// An anonymous function. It has no name, in the same way that the string<br>// &quot;hello&quot; has no name. Since it is an expression, we can give it a temporary<br>// name by assigning it to the variable $triple.<br></span><span class="default">$triple </span><span class="keyword">= function(</span><span class="default">$x</span><span class="keyword">) {<br>&#xA0; return </span><span class="default">3 </span><span class="keyword">* </span><span class="default">$x</span><span class="keyword">;<br>};<br></span><span class="default">?&gt;<br></span><br>We can &quot;call&quot; (or &quot;run&quot;) both kinds of function. A &quot;function call&quot; is an<br>expression with the value of whatever the function returns. For example:<br><br><span class="default">&lt;?php<br></span><span class="comment">// The easiest way to run a function is to put () after its name, containing its<br>// arguments (if any)<br></span><span class="default">$my_numbers </span><span class="keyword">= array(</span><span class="default">double</span><span class="keyword">(</span><span class="default">5</span><span class="keyword">), </span><span class="default">$triple</span><span class="keyword">(</span><span class="default">5</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>$my_numbers is now an array containing 10 and 15, which are the return values of<br>double and $triple when applied to the number 5.<br><br>Importantly, if we *don&apos;t* call a function, ie. we don&apos;t put () after its name,<br>then we still get expressions. For example:<br><br><span class="default">&lt;?php<br>$my_functions </span><span class="keyword">= array(</span><span class="string">&apos;double&apos;</span><span class="keyword">, </span><span class="default">$triple</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>$my_functions is now an array containing these two functions. Notice that named<br>functions are more awkward than anonymous functions. PHP treats them differently<br>because it didn&apos;t use to have anonymous functions, and the way named functions<br>were implemented didn&apos;t work for anonymous functions when they were eventually<br>added.<br><br>This means that instead of using a named function literally, like we can with<br>anonymous functions, we have to use a string containing its name instead. PHP<br>makes sure that these strings will be treated as functions when it&apos;s<br>appropriate. For example:<br><br><span class="default">&lt;?php<br>$temp&#xA0; &#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&apos;double&apos;</span><span class="keyword">;<br></span><span class="default">$my_number </span><span class="keyword">= </span><span class="default">$temp</span><span class="keyword">(</span><span class="default">5</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>$my_number will be 10, since PHP has spotted that we&apos;re treating a string as if<br>it were a function, so it has looked up that named function for us.<br><br>Unfortunately PHP&apos;s parser is very quirky; rather than looking for generic<br>patterns like &quot;x(y)&quot; and seeing if &quot;x&quot; is a function, it has lots of<br>special-cases like &quot;$x(y)&quot;. This makes code like &quot;&apos;double&apos;(5)&quot; invalid, so we<br>have to do tricks like using temporary variables. There is another way around<br>this restriction though, and that is to pass our functions to the<br>&quot;call_user_func&quot; or &quot;call_user_func_array&quot; functions when we want to call them.<br>For example:<br><br><span class="default">&lt;?php<br>$my_numbers </span><span class="keyword">= array(</span><span class="default">call_user_func</span><span class="keyword">(</span><span class="string">&apos;double&apos;</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">), </span><span class="default">call_user_func</span><span class="keyword">(</span><span class="default">$triple</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>$my_numbers contains 10 and 15 because &quot;call_user_func&quot; called our functions for<br>us. This is possible because the string &apos;double&apos; and the anonymous function<br>$triple are expressions. Note that we can even use this technique to call an<br>anonymous function without ever giving it a name:<br><br><span class="default">&lt;?php<br>$my_number </span><span class="keyword">= </span><span class="default">call_user_func</span><span class="keyword">(function(</span><span class="default">$x</span><span class="keyword">) { return </span><span class="default">4 </span><span class="keyword">* </span><span class="default">$x</span><span class="keyword">; }, </span><span class="default">5</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>$my_number is now 20, since &quot;call_user_func&quot; called the anonymous function,<br>which quadruples its argument, with the value 5.<br><br>Passing functions around as expressions like this is very useful whenever we<br>need to use a &apos;callback&apos;. Great examples of this are array_map and array_reduce.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.expressions.php)

**[To root](/)**