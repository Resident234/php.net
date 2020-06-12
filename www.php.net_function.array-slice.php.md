# array_slice




<div class="phpcode"><span class="html">
Array slice function that works with associative arrays (keys):<br><br>function array_slice_assoc($array,$keys) {<br>&#xA0; &#xA0; return array_intersect_key($array,array_flip($keys));<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
array_slice can be used to remove elements from an array but it&apos;s pretty simple to use a custom function.<br><br>One day array_remove() might become part of PHP and will likely be a reserved function name, hence the unobvious choice for this function&apos;s names.<br><br>&lt;?<br>function arem($array,$value){<br>&#xA0; &#xA0; $holding=array();<br>&#xA0; &#xA0; foreach($array as $k =&gt; $v){<br>&#xA0; &#xA0; &#xA0; &#xA0; if($value!=$v){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $holding[$k]=$v;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }&#xA0; &#xA0; <br>&#xA0; &#xA0; return $holding;<br>}<br><br>function akrem($array,$key){<br>&#xA0; &#xA0; $holding=array();<br>&#xA0; &#xA0; foreach($array as $k =&gt; $v){<br>&#xA0; &#xA0; &#xA0; &#xA0; if($key!=$k){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $holding[$k]=$v;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }&#xA0; &#xA0; <br>&#xA0; &#xA0; return $holding;<br>}<br><br>$lunch = array(&apos;sandwich&apos; =&gt; &apos;cheese&apos;, &apos;cookie&apos;=&gt;&apos;oatmeal&apos;,&apos;drink&apos; =&gt; &apos;tea&apos;,&apos;fruit&apos; =&gt; &apos;apple&apos;);<br>echo &apos;&lt;pre&gt;&apos;;<br>print_r($lunch);<br>$lunch=arem($lunch,&apos;apple&apos;);<br>print_r($lunch);<br>$lunch=akrem($lunch,&apos;sandwich&apos;);<br>print_r($lunch);<br>echo &apos;&lt;/pre&gt;&apos;;<br>?&gt;<br><br>(remove 9&apos;s in email)</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">// CHOP $num ELEMENTS OFF THE FRONT OF AN ARRAY<br>// RETURN THE CHOP, SHORTENING THE SUBJECT ARRAY<br></span><span class="keyword">function </span><span class="default">array_chop</span><span class="keyword">(&amp;</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$ret </span><span class="keyword">= </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$arr </span><span class="keyword">= </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">, </span><span class="default">$num</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$ret</span><span class="keyword">;<br>}</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-slice.php)

**[⬆ to root](/)**