# array_reduce




<div class="phpcode"><span class="html">
To make it clearer about what the two parameters of the callback are for, and what &quot;reduce to a single value&quot; actually means (using associative and commutative operators as examples may obscure this).<br><br>The first parameter to the callback is an accumulator where the result-in-progress is effectively assembled. If you supply an $initial value the accumulator starts out with that value, otherwise it starts out null.<br>The second parameter is where each value of the array is passed during each step of the reduction.<br>The return value of the callback becomes the new value of the accumulator. When the array is exhausted, array_reduce() returns accumulated value.<br><br>If you carried out the reduction by hand, you&apos;d get something like the following lines, every one of which therefore producing the same result:<br><span class="default">&lt;?php<br>array_reduce</span><span class="keyword">(array(</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">), </span><span class="string">&apos;f&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">99&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">);<br></span><span class="default">array_reduce</span><span class="keyword">(array(</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">),&#xA0;&#xA0; </span><span class="string">&apos;f&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">f</span><span class="keyword">(</span><span class="default">99</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );<br></span><span class="default">array_reduce</span><span class="keyword">(array(</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">),&#xA0; &#xA0;&#xA0; </span><span class="string">&apos;f&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">99</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">),</span><span class="default">2</span><span class="keyword">)&#xA0; &#xA0; &#xA0;&#xA0; );<br></span><span class="default">array_reduce</span><span class="keyword">(array(</span><span class="default">4</span><span class="keyword">),&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&apos;f&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">99</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">),</span><span class="default">2</span><span class="keyword">),</span><span class="default">3</span><span class="keyword">)&#xA0; &#xA0; );<br></span><span class="default">array_reduce</span><span class="keyword">(array(),&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;f&apos;</span><span class="keyword">, </span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">99</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">),</span><span class="default">2</span><span class="keyword">),</span><span class="default">3</span><span class="keyword">),</span><span class="default">4</span><span class="keyword">) );<br></span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">f</span><span class="keyword">(</span><span class="default">99</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">),</span><span class="default">2</span><span class="keyword">),</span><span class="default">3</span><span class="keyword">),</span><span class="default">4</span><span class="keyword">)<br></span><span class="default">?&gt;<br></span><br>If you made function f($v,$w){return &quot;f($v,$w)&quot;;} the last line would be the literal result.<br><br>A PHP implementation might therefore look something like this (less details like error checking and so on):<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_reduce</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$callback</span><span class="keyword">, </span><span class="default">$initial</span><span class="keyword">=</span><span class="default">null</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$acc </span><span class="keyword">= </span><span class="default">$initial</span><span class="keyword">;<br>&#xA0; &#xA0; foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$a</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$acc </span><span class="keyword">= </span><span class="default">$callback</span><span class="keyword">(</span><span class="default">$acc</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$acc</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sometimes we need to go through an array and group the indexes so that it is easier and easier to extract them in the iteration.<br><br><span class="default">&lt;?php<br><br>$people </span><span class="keyword">= [<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Hayley&apos;</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Jack&apos;</span><span class="keyword">, </span><span class="string">&apos;dad&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Linus&apos;</span><span class="keyword">, </span><span class="string">&apos;dad&apos;</span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Peter&apos; </span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">5</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Tom&apos;</span><span class="keyword">, </span><span class="string">&apos;dad&apos; </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">],<br>];<br><br></span><span class="default">$family </span><span class="keyword">= </span><span class="default">array_reduce</span><span class="keyword">(</span><span class="default">$people</span><span class="keyword">, function(</span><span class="default">$accumulator</span><span class="keyword">, </span><span class="default">$item</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// if you don&apos;t have a dad you are probably a dad<br>&#xA0; &#xA0; </span><span class="keyword">if (!isset(</span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;dad&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$id </span><span class="keyword">= </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;id&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$name </span><span class="keyword">= </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;name&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// take the children if you already have <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$children </span><span class="keyword">= </span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$id</span><span class="keyword">][</span><span class="string">&apos;children&apos;</span><span class="keyword">] ?? [];<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// add dad<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$id</span><span class="keyword">] = [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">$id</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="default">$name</span><span class="keyword">,</span><span class="string">&apos;children&apos; </span><span class="keyword">=&gt; </span><span class="default">$children</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$accumulator</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">// add a new dad if you haven&apos;t already <br>&#xA0; &#xA0; </span><span class="default">$dad </span><span class="keyword">= </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;dad&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; if (!isset(</span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$dad</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// how did you find the dad will first add only with children <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$dad</span><span class="keyword">] = [</span><span class="string">&apos;children&apos; </span><span class="keyword">=&gt; [</span><span class="default">$item</span><span class="keyword">]];<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$accumulator</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">//&#xA0; add a son to his dad who has already been added<br>&#xA0; &#xA0; //&#xA0; by the first or second conditional &quot;if&quot;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$dad</span><span class="keyword">][</span><span class="string">&apos;children&apos;</span><span class="keyword">][] = </span><span class="default">$item</span><span class="keyword">;<br>&#xA0; &#xA0; return </span><span class="default">$accumulator</span><span class="keyword">;<br>}, []);<br><br></span><span class="default">var_export</span><span class="keyword">(</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$family</span><span class="keyword">));<br><br></span><span class="default">?&gt;<br></span><br>OUTPUT<br><br>array (<br>&#xA0; 0 =&gt;<br>&#xA0; array (<br>&#xA0; &#xA0; &apos;id&apos; =&gt; 1,<br>&#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Hayley&apos;,<br>&#xA0; &#xA0; &apos;children&apos; =&gt;<br>&#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; 0 =&gt;<br>&#xA0; &#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 2,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Jack&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;dad&apos; =&gt; 1,<br>&#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; ),<br>&#xA0; ),<br>&#xA0; 1 =&gt;<br>&#xA0; array (<br>&#xA0; &#xA0; &apos;id&apos; =&gt; 4,<br>&#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Peter&apos;,<br>&#xA0; &#xA0; &apos;children&apos; =&gt;<br>&#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; 0 =&gt;<br>&#xA0; &#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 3,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Linus&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;dad&apos; =&gt; 4,<br>&#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; 1 =&gt;<br>&#xA0; &#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 5,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;name&apos; =&gt; &apos;Tom&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;dad&apos; =&gt; 4,<br>&#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; ),<br>&#xA0; ),<br>)<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= [<br>&#xA0; [<br>&#xA0; &#xA0; </span><span class="string">&quot;menu_id&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;1&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;menu_name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;Clients&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;submenu_name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;Add&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;submenu_link&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;clients/add&quot;<br>&#xA0; </span><span class="keyword">],<br>&#xA0; [<br>&#xA0; &#xA0; </span><span class="string">&quot;menu_id&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;1&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;menu_name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;Clients&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;submenu_name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;List&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;submenu_link&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;clients&quot;<br>&#xA0; </span><span class="keyword">],<br>&#xA0; [<br>&#xA0; &#xA0; </span><span class="string">&quot;menu_id&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;2&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;menu_name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;Products&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;submenu_name&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;List&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;submenu_link&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;products&quot;<br>&#xA0; </span><span class="keyword">],<br>];<br><br></span><span class="comment">//Grouping submenus to their menus<br><br></span><span class="default">$menu </span><span class="keyword">=&#xA0; </span><span class="default">array_reduce</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, function(</span><span class="default">$accumulator</span><span class="keyword">, </span><span class="default">$item</span><span class="keyword">){<br>&#xA0; </span><span class="default">$index </span><span class="keyword">= </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;menu_name&apos;</span><span class="keyword">];<br><br>&#xA0; if (!isset(</span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">])) {<br>&#xA0; &#xA0; </span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">] = [<br>&#xA0; &#xA0; &#xA0; </span><span class="string">&apos;menu_id&apos; </span><span class="keyword">=&gt; </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;menu_id&apos;</span><span class="keyword">],<br>&#xA0; &#xA0; &#xA0; </span><span class="string">&apos;menu_name&apos; </span><span class="keyword">=&gt; </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;menu_name&apos;</span><span class="keyword">],<br>&#xA0; &#xA0; &#xA0; </span><span class="string">&apos;submenu&apos; </span><span class="keyword">=&gt; []&#xA0; &#xA0; <br>&#xA0; &#xA0; ];<br>&#xA0; }<br><br>&#xA0; </span><span class="default">$accumulator</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">][</span><span class="string">&apos;submenu&apos;</span><span class="keyword">][] = [<br>&#xA0; &#xA0; </span><span class="string">&apos;submenu_name&apos; </span><span class="keyword">=&gt; </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;submenu_name&apos;</span><span class="keyword">],<br>&#xA0; &#xA0; </span><span class="string">&apos;submenu_link&apos; </span><span class="keyword">=&gt; </span><span class="default">$item</span><span class="keyword">[</span><span class="string">&apos;submenu_link&apos;</span><span class="keyword">]<br>&#xA0; ];<br><br>&#xA0; return </span><span class="default">$accumulator</span><span class="keyword">;<br>}, []);<br><br></span><span class="default">var_export</span><span class="keyword">(</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$menu</span><span class="keyword">));<br><br></span><span class="default">?&gt;<br></span><br>OUTPUT<br><br>array (<br>&#xA0; 0 =&gt;<br>&#xA0; array (<br>&#xA0; &#xA0; &apos;menu_id&apos; =&gt; &apos;1&apos;,<br>&#xA0; &#xA0; &apos;menu_name&apos; =&gt; &apos;Clients&apos;,<br>&#xA0; &#xA0; &apos;submenu&apos; =&gt;<br>&#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; 0 =&gt;<br>&#xA0; &#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;submenu_name&apos; =&gt; &apos;Add&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;submenu_link&apos; =&gt; &apos;clients/add&apos;,<br>&#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; &#xA0; 1 =&gt;<br>&#xA0; &#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;submenu_name&apos; =&gt; &apos;List&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;submenu_link&apos; =&gt; &apos;clients&apos;,<br>&#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; ),<br>&#xA0; ),<br>&#xA0; 1 =&gt;<br>&#xA0; array (<br>&#xA0; &#xA0; &apos;menu_id&apos; =&gt; &apos;2&apos;,<br>&#xA0; &#xA0; &apos;menu_name&apos; =&gt; &apos;Products&apos;,<br>&#xA0; &#xA0; &apos;submenu&apos; =&gt;<br>&#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; 0 =&gt;<br>&#xA0; &#xA0; &#xA0; array (<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;submenu_name&apos; =&gt; &apos;List&apos;,<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;submenu_link&apos; =&gt; &apos;products&apos;,<br>&#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; ),<br>&#xA0; ),<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
So, if you were wondering how to use this where key and value are passed in to the function. I&apos;ve had success with the following (this example generates formatted html attributes from an associative array of attribute =&gt; value pairs):<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="comment">// Attribute List<br>&#xA0; &#xA0; </span><span class="default">$attribs </span><span class="keyword">= [<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;first_name&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;value&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Edward&apos;<br>&#xA0; &#xA0; </span><span class="keyword">];<br><br>&#xA0; &#xA0; </span><span class="comment">// Attribute string formatted for use inside HTML element<br>&#xA0; &#xA0; </span><span class="default">$formatted_attribs </span><span class="keyword">= </span><span class="default">array_reduce</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$attribs</span><span class="keyword">),&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// We pass in the array_keys instead of the array here<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">function (</span><span class="default">$carry</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">) use (</span><span class="default">$attribs</span><span class="keyword">) {&#xA0; &#xA0; </span><span class="comment">// ... then we &apos;use&apos; the actual array here<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$carry </span><span class="keyword">. </span><span class="string">&apos; &apos; </span><span class="keyword">. </span><span class="default">$key </span><span class="keyword">. </span><span class="string">&apos;=&quot;&apos; </span><span class="keyword">. </span><span class="default">htmlspecialchars</span><span class="keyword">( </span><span class="default">$attribs</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] ) . </span><span class="string">&apos;&quot;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; },<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;&apos;<br>&#xA0; &#xA0; </span><span class="keyword">);<br><br>echo </span><span class="default">$formatted_attribs</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>This will output:<br> name=&quot;first_name&quot; value=&quot;Edward&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can effectively ignore the fact $result is passed into the callback by reference. Only the return value of the callback is accounted for.<br><br><span class="default">&lt;?php<br><br>$arr </span><span class="keyword">= [</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">];<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_reduce</span><span class="keyword">(<br>&#xA0; &#xA0; </span><span class="default">$arr</span><span class="keyword">,<br>&#xA0; &#xA0; function(&amp;</span><span class="default">$res</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">) { </span><span class="default">$res </span><span class="keyword">+= </span><span class="default">$a</span><span class="keyword">; }, <br>&#xA0; &#xA0; </span><span class="default">0<br></span><span class="keyword">));<br><br></span><span class="comment"># NULL<br><br></span><span class="default">?&gt;<br></span><br><span class="default">&lt;?php<br><br>$arr </span><span class="keyword">= [</span><span class="default">1</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">,</span><span class="default">4</span><span class="keyword">];<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_reduce</span><span class="keyword">(<br>&#xA0; &#xA0; </span><span class="default">$arr</span><span class="keyword">, <br>&#xA0; &#xA0; function(</span><span class="default">$res</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">) { return </span><span class="default">$res </span><span class="keyword">+ </span><span class="default">$a</span><span class="keyword">;&#xA0; }, <br>&#xA0; &#xA0; </span><span class="default">0<br></span><span class="keyword">));<br><br></span><span class="comment"># int(10)<br></span><span class="default">?&gt;<br></span><br>Be warned, though, that you *can* accidentally change $res if it&apos;s not a simple scalar value, so despite the examples I&apos;d recommend not writing to it at all.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you do not provide $initial, the first value used in the iteration is NULL. This is not a problem for callback functions that treat NULL as an identity (e.g. addition), but is a problem for cases when NULL is not identity (such as boolean context).
<br>
<br>Compare:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">andFunc</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) {
<br>&#xA0; return </span><span class="default">$a </span><span class="keyword">&amp;&amp; </span><span class="default">$b</span><span class="keyword">;
<br>}
<br></span><span class="default">$foo </span><span class="keyword">= array(</span><span class="default">true</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">array_reduce</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">, </span><span class="string">&quot;andFunc&quot;</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>returns false! One would expect that it would return true because `true &amp;&amp; true &amp;&amp; true == true`!
<br>
<br>Adding diagnostic output to andFunc() shows that the first call to andFunc is with the arguments (NULL, true). This resolves to false (as `(bool) null == false`) and thereby corrupts the whole reduction.
<br>
<br>So in this case I have to set `$initial = true` so that the first call to andFunc() will be (true, true). Now, if I were doing, say, orFunc(), I would have to set `$initial = false`. Beware.
<br>
<br>Note that the &quot;rmul&quot; case in the example sneakily hides this defect! They use an $initial of 10 to get `10*1*2*3*4*5 = 12000`. So you would assume that without an initial, you would get `1200/10 = 120 = 1*2*3*4*5`. Nope! You get big fat zero, because `int(null)==0`, and `0*1*2*3*4*5 = 0`!
<br>
<br>I don&apos;t honestly see why array_reduce starts with a null argument. The first call to the callback should be with arguments ($initial[0],$initial[1]) [or whatever the first two array entries are], not (null,$initial[0]). That&apos;s what one would expect from the description.
<br>
<br>Incidentally this also means that under the current implementation you will incur `count($input)` number of calls to the callback, not `count($input) - 1` as you might expect.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-reduce.php)

**[To root](/README.md)**