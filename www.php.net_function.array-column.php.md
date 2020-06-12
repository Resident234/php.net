# array_column




<div class="phpcode"><span class="html">
if array_column does not exist the below solution will work.<br><br>if(!function_exists(&quot;array_column&quot;))<br>{<br><br>&#xA0; &#xA0; function array_column($array,$column_name)<br>&#xA0; &#xA0; {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return array_map(function($element) use($column_name){return $element[$column_name];}, $array);<br><br>&#xA0; &#xA0; }<br><br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can also use array_map fucntion if you haven&apos;t array_column().
<br>
<br>example:
<br>
<br>$a = array(
<br>&#xA0; &#xA0; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 2135,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;first_name&apos; =&gt; &apos;John&apos;,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;last_name&apos; =&gt; &apos;Doe&apos;,
<br>&#xA0; &#xA0; ),
<br>&#xA0; &#xA0; array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;id&apos; =&gt; 3245,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;first_name&apos; =&gt; &apos;Sally&apos;,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &apos;last_name&apos; =&gt; &apos;Smith&apos;,
<br>&#xA0; &#xA0; )
<br>);
<br>
<br>array_column($a, &apos;last_name&apos;);
<br>
<br>becomes
<br>
<br>array_map(function($element){return $element[&apos;last_name&apos;];}, $a);</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function does not preserve the original keys of the array (when not providing an index_key).<br><br>You can work around that like so:<br><br><span class="default">&lt;?php<br></span><span class="comment">// instead of<br></span><span class="default">array_column</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="string">&apos;column&apos;</span><span class="keyword">);<br><br></span><span class="comment">// to preserve keys<br></span><span class="default">array_combine</span><span class="keyword">(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">), </span><span class="default">array_column</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="string">&apos;column&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some remarks not included in the official documentation.<br><br>1) array_column does not support 1D arrays, in which case an empty array is returned.<br><br>2) The $column_key is zero-based.<br><br>3) If $column_key extends the valid index range an empty array is returned.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-column.php)

**[⬆ to root](/)**