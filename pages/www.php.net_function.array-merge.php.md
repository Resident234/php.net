# array_merge




<div class="phpcode"><span class="html">
In some situations, the union operator ( + ) might be more useful to you than array_merge.&#xA0; The array_merge function does not preserve numeric key values.&#xA0; If you need to preserve the numeric keys, then using + will do that.<br><br>ie:<br><br><span class="default">&lt;?php<br><br>$array1</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="string">&quot;zero&quot;</span><span class="keyword">;<br></span><span class="default">$array1</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="string">&quot;one&quot;</span><span class="keyword">;<br><br></span><span class="default">$array2</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="string">&quot;one&quot;</span><span class="keyword">;<br></span><span class="default">$array2</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = </span><span class="string">&quot;two&quot;</span><span class="keyword">;<br></span><span class="default">$array2</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] = </span><span class="string">&quot;three&quot;</span><span class="keyword">;<br><br></span><span class="default">$array3 </span><span class="keyword">= </span><span class="default">$array1 </span><span class="keyword">+ </span><span class="default">$array2</span><span class="keyword">;<br><br></span><span class="comment">//This will result in::<br><br></span><span class="default">$array3 </span><span class="keyword">= array(</span><span class="default">0</span><span class="keyword">=&gt;</span><span class="string">&quot;zero&quot;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">=&gt;</span><span class="string">&quot;one&quot;</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">=&gt;</span><span class="string">&quot;two&quot;</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">=&gt;</span><span class="string">&quot;three&quot;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Note the implicit &quot;array_unique&quot; that gets applied as well.&#xA0; In some situations where your numeric keys matter, this behaviour could be useful, and better than array_merge.<br><br>--Julian</span>
</div>
  

#


<div class="phpcode"><span class="html">
As PHP 5.6 you can use array_merge + &quot;splat&quot; operator to reduce a bidimensonal array to a simple array:<br><br><span class="default">&lt;?php<br>$data </span><span class="keyword">= [[</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">], [</span><span class="default">3</span><span class="keyword">], [</span><span class="default">4</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">]];<br>&#xA0; &#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(... </span><span class="default">$data</span><span class="keyword">)); </span><span class="comment">// [1, 2, 3, 4, 5];<br></span><span class="default">?&gt;<br></span><br>PHP &lt; 5.6:<br><br><span class="default">&lt;?php<br>$data </span><span class="keyword">= [[</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">], [</span><span class="default">3</span><span class="keyword">], [</span><span class="default">4</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">]];<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_reduce</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">, </span><span class="string">&apos;array_merge&apos;</span><span class="keyword">, []));&#xA0;&#xA0; </span><span class="comment">// [1, 2, 3, 4, 5];<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sometimes we need to traverse an array and group / merge the indexes so that it is easier to extract them so that they are related in the iteration.<br><br><span class="default">&lt;?php<br><br>$people </span><span class="keyword">= [<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Hayley&apos;</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Jack&apos;</span><span class="keyword">, </span><span class="string">&apos;dad&apos; </span><span class="keyword">=&gt; </span><span class="default">1</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">3</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Linus&apos;</span><span class="keyword">, </span><span class="string">&apos;dad&apos; </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Peter&apos;</span><span class="keyword">],<br>&#xA0; &#xA0; [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">5</span><span class="keyword">, </span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Tom&apos;</span><span class="keyword">, </span><span class="string">&apos;dad&apos; </span><span class="keyword">=&gt; </span><span class="default">4</span><span class="keyword">],<br>];<br><br></span><span class="comment">// We set up an array with just the children<br></span><span class="keyword">function </span><span class="default">children</span><span class="keyword">(</span><span class="default">$dad</span><span class="keyword">, </span><span class="default">$people</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$children </span><span class="keyword">= [];<br>&#xA0; &#xA0; foreach (</span><span class="default">$people </span><span class="keyword">as </span><span class="default">$p</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!empty(</span><span class="default">$p</span><span class="keyword">[</span><span class="string">&quot;dad&quot;</span><span class="keyword">]) &amp;&amp; </span><span class="default">$p</span><span class="keyword">[</span><span class="string">&quot;dad&quot;</span><span class="keyword">] == </span><span class="default">$dad</span><span class="keyword">[</span><span class="string">&quot;id&quot;</span><span class="keyword">]) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$children</span><span class="keyword">[] = </span><span class="default">$p</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return </span><span class="default">$children</span><span class="keyword">;<br>}<br><br></span><span class="default">$family </span><span class="keyword">= [];<br><br></span><span class="comment">// We merge each child with its respective parent<br></span><span class="keyword">foreach (</span><span class="default">$people </span><span class="keyword">as </span><span class="default">$p</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$children </span><span class="keyword">= </span><span class="default">children</span><span class="keyword">(</span><span class="default">$p</span><span class="keyword">, </span><span class="default">$people</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">$children </span><span class="keyword">!= []) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$family</span><span class="keyword">[] = </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$p</span><span class="keyword">, [</span><span class="string">&quot;children&quot; </span><span class="keyword">=&gt; </span><span class="default">$children</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$family</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>//OUTPUT<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Hayley<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [children] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Jack<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [dad] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Peter<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [children] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Linus<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [dad] =&gt; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 5<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Tom<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [dad] =&gt; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
An addition to what Julian Egelstaff above wrote - the array union operation (+) is not doing an array_unique - it will just not use the keys that are already defined in the left array.&#xA0; The difference between union and merge can be seen in an example like this:<br><br><span class="default">&lt;?php<br>$arr1</span><span class="keyword">[</span><span class="string">&apos;one&apos;</span><span class="keyword">] = </span><span class="string">&apos;one&apos;</span><span class="keyword">;<br></span><span class="default">$arr1</span><span class="keyword">[</span><span class="string">&apos;two&apos;</span><span class="keyword">] = </span><span class="string">&apos;two&apos;</span><span class="keyword">;<br><br></span><span class="default">$arr2</span><span class="keyword">[</span><span class="string">&apos;zero&apos;</span><span class="keyword">] = </span><span class="string">&apos;zero&apos;</span><span class="keyword">;<br></span><span class="default">$arr2</span><span class="keyword">[</span><span class="string">&apos;one&apos;</span><span class="keyword">] = </span><span class="string">&apos;three&apos;</span><span class="keyword">;<br></span><span class="default">$arr2</span><span class="keyword">[</span><span class="string">&apos;two&apos;</span><span class="keyword">] = </span><span class="string">&apos;four&apos;</span><span class="keyword">;<br><br></span><span class="default">$arr3 </span><span class="keyword">= </span><span class="default">$arr1 </span><span class="keyword">+ </span><span class="default">$arr2</span><span class="keyword">;<br></span><span class="default">var_export</span><span class="keyword">( </span><span class="default">$arr3 </span><span class="keyword">);<br></span><span class="comment"># array ( &apos;one&apos; =&gt; &apos;one&apos;, &apos;two&apos; =&gt; &apos;two&apos;, &apos;zero&apos; =&gt; &apos;zero&apos;, )<br><br></span><span class="default">$arr4 </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">( </span><span class="default">$arr1</span><span class="keyword">, </span><span class="default">$arr2 </span><span class="keyword">);<br></span><span class="default">var_export</span><span class="keyword">( </span><span class="default">$arr4 </span><span class="keyword">);<br></span><span class="comment"># array ( &apos;one&apos; =&gt; &apos;three&apos;, &apos;two&apos; =&gt; &apos;four&apos;, &apos;zero&apos; =&gt; &apos;zero&apos;, )<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Reiterating the notes about casting to arrays, be sure to cast if one of the arrays might be null:<br><br><span class="default">&lt;?php<br>header</span><span class="keyword">(</span><span class="string">&quot;Content-type:text/plain&quot;</span><span class="keyword">);<br></span><span class="default">$a </span><span class="keyword">= array(</span><span class="string">&apos;zzzz&apos;</span><span class="keyword">, </span><span class="string">&apos;xxxx&apos;</span><span class="keyword">);<br></span><span class="default">$b </span><span class="keyword">= array(</span><span class="string">&apos;mmmm&apos;</span><span class="keyword">,</span><span class="string">&apos;nnnn&apos;</span><span class="keyword">);<br><br>echo </span><span class="string">&quot;1 ==============\r\n&quot;</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">));<br><br>echo </span><span class="string">&quot;2 ==============\r\n&quot;</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= array();<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">));<br><br>echo </span><span class="string">&quot;3 ==============\r\n&quot;</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">));<br><br>echo </span><span class="string">&quot;4 ==============\r\n&quot;</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, (array)</span><span class="default">$b</span><span class="keyword">));<br><br>echo </span><span class="string">&quot;5 ==============\r\n&quot;</span><span class="keyword">;<br>echo </span><span class="default">is_null</span><span class="keyword">(</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)) ? </span><span class="string">&apos;Result is null&apos; </span><span class="keyword">: </span><span class="string">&apos;Result is not null&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Produces:<br><br>1 ==============<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; zzzz<br>&#xA0; &#xA0; [1] =&gt; xxxx<br>&#xA0; &#xA0; [2] =&gt; mmmm<br>&#xA0; &#xA0; [3] =&gt; nnnn<br>)<br>2 ==============<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; zzzz<br>&#xA0; &#xA0; [1] =&gt; xxxx<br>)<br>3 ==============<br>4 ==============<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; zzzz<br>&#xA0; &#xA0; [1] =&gt; xxxx<br>)<br>5 ==============<br>Result is null</span>
</div>
  

#


<div class="phpcode"><span class="html">
to get unique value from multi dimensional array use this instead of array_unique(), because array_unique() does not work on multidimensional:<br>array_map(&quot;unserialize&quot;, array_unique(array_map(&quot;serialize&quot;, $array)));<br>Hope this will help someone;<br>Example<br>$a=array(array(&apos;1&apos;),array(&apos;2&apos;),array(&apos;3&apos;),array(&apos;4));<br>$b=array(array(&apos;2&apos;),array(&apos;4&apos;),array(&apos;6&apos;),array(&apos;8));<br>$c=array_merge($a,$b);<br>then write this line to get unique values<br>$c=array_map(&quot;unserialize&quot;, array_unique(array_map(&quot;serialize&quot;, $c)));<br>print_r($c);</span>
</div>
  

#


<div class="phpcode"><span class="html">
I constantly forget the direction of array_merge so this is partially for me and partially for people like me.<br><br>array_merge is a non-referential non-inplace right-reduction. For different ctrl-f typers, it&apos;s reduce-right, side-effect free, idempotent, and non in-place.<br><br>ex:<br><br>$a = array_merge([&apos;k&apos; =&gt; &apos;a&apos;], [&apos;k&apos; =&gt; &apos;b&apos;]) =&gt; [&apos;k&apos; =&gt; &apos;b&apos;]<br>array_merge([&apos;z&apos; =&gt; 1], $a) =&gt; does not modify $a but returns [&apos;k&apos; =&gt; &apos;b&apos;, &apos;z&apos; =&gt; 1];<br><br>Hopefully this helps people that constant look this up such as myself.</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you generate form select from an array, you probably want to keep your array keys and order intact,<br>if so you can use ArrayMergeKeepKeys(), works just like array_merge :<br><br>array ArrayMergeKeepKeys ( array array1 [, array array2 [, array ...]])<br><br>but keeps the keys even if of numeric kind. <br>enjoy<br><br>&lt;?<br><br>$Default[0]=&apos;Select Something please&apos;;<br><br>$Data[147]=&apos;potato&apos;;<br>$Data[258]=&apos;banana&apos;;<br>$Data[54]=&apos;tomato&apos;;<br><br>$A=array_merge($Default,$Data);<br><br>$B=ArrayMergeKeepKeys($Default,$Data);<br><br>echo &apos;&lt;pre&gt;&apos;;<br>print_r($A);<br>print_r($B);<br>echo &apos;&lt;/pre&gt;&apos;;<br><br>Function ArrayMergeKeepKeys() {<br>&#xA0; &#xA0; &#xA0; $arg_list = func_get_args();<br>&#xA0; &#xA0; &#xA0; foreach((array)$arg_list as $arg){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach((array)$arg as $K =&gt; $V){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $Zoo[$K]=$V;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; return $Zoo;<br>}<br><br>//will output :<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Select Something please<br>&#xA0; &#xA0; [1] =&gt; potato<br>&#xA0; &#xA0; [2] =&gt; banana<br>&#xA0; &#xA0; [3] =&gt; tomato<br>)<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Select Something please<br>&#xA0; &#xA0; [147] =&gt; potato<br>&#xA0; &#xA0; [258] =&gt; banana<br>&#xA0; &#xA0; [54] =&gt; tomato<br>)<br><br>?&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
I keep seeing posts for people looking for a function to replace numeric keys.
<br>
<br>No function is required for this, it is default behavior if the + operator:
<br>
<br><span class="default">&lt;?php
<br>$a</span><span class="keyword">=array(</span><span class="default">1</span><span class="keyword">=&gt;</span><span class="string">&quot;one&quot;</span><span class="keyword">, </span><span class="string">&quot;two&quot;</span><span class="keyword">=&gt;</span><span class="default">2</span><span class="keyword">);
<br></span><span class="default">$b</span><span class="keyword">=array(</span><span class="default">1</span><span class="keyword">=&gt;</span><span class="string">&quot;two&quot;</span><span class="keyword">, </span><span class="string">&quot;two&quot;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">=&gt;</span><span class="string">&quot;three&quot;</span><span class="keyword">, </span><span class="string">&quot;four&quot;</span><span class="keyword">=&gt;</span><span class="default">4</span><span class="keyword">);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">+</span><span class="default">$b</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Array
<br>(
<br>&#xA0; &#xA0; [1] =&gt; one
<br>&#xA0; &#xA0; [two] =&gt; 2
<br>&#xA0; &#xA0; [3] =&gt; three
<br>&#xA0; &#xA0; [four] =&gt; 4
<br>)
<br>
<br>How this works:
<br>
<br>The + operator only adds unique keys to the resulting array.&#xA0; By making the replacements the first argument, they naturally always replace the keys from the second argument, numeric or not! =)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-merge.php)

**[To root](/README.md)**