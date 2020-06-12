# array_diff_assoc




<div class="phpcode"><span class="html">
The array_diff_assoc_array from &quot;chinello at gmail dot com&quot; (and others) will not work for arrays with null values. That&apos;s because !isset is true when an array key doesn&apos;t exists or is set to null.<br><br>(sorry for the changed indent-style)<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">array_diff_assoc_recursive</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">=array();<br>&#xA0; &#xA0; foreach(</span><span class="default">$array1 </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if( </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if( !isset(</span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]) || !</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$new_diff </span><span class="keyword">= </span><span class="default">array_diff_assoc_recursive</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if( !empty(</span><span class="default">$new_diff</span><span class="keyword">) )<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$new_diff</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; } else if( !</span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">,</span><span class="default">$array2</span><span class="keyword">) || </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] !== </span><span class="default">$value </span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$difference</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>And here an example (note index &apos;b&apos; in the output):<br><span class="default">&lt;?php<br>$a1</span><span class="keyword">=array( </span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">null</span><span class="keyword">, </span><span class="string">&apos;c&apos; </span><span class="keyword">=&gt; array( </span><span class="string">&apos;d&apos; </span><span class="keyword">=&gt; </span><span class="default">null </span><span class="keyword">) );<br></span><span class="default">$a2</span><span class="keyword">=array( </span><span class="string">&apos;a&apos; </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">, </span><span class="string">&apos;b&apos; </span><span class="keyword">=&gt; </span><span class="default">null </span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">( </span><span class="default">array_diff_assoc_recursive</span><span class="keyword">( </span><span class="default">$a1</span><span class="keyword">, </span><span class="default">$a2 </span><span class="keyword">) );<br></span><span class="default">var_dump</span><span class="keyword">( </span><span class="default">chinello_array_diff_assoc_recursive</span><span class="keyword">( </span><span class="default">$a1</span><span class="keyword">, </span><span class="default">$a2 </span><span class="keyword">) );<br></span><span class="default">?&gt;<br></span>array(1) {<br>&#xA0; [&quot;c&quot;]=&gt;<br>&#xA0; array(1) {<br>&#xA0; &#xA0; [&quot;d&quot;]=&gt;<br>&#xA0; &#xA0; NULL<br>&#xA0; }<br>}<br><br>array(2) {<br>&#xA0; [&quot;b&quot;]=&gt;<br>&#xA0; NULL<br>&#xA0; [&quot;c&quot;]=&gt;<br>&#xA0; array(1) {<br>&#xA0; &#xA0; [&quot;d&quot;]=&gt;<br>&#xA0; &#xA0; NULL<br>&#xA0; }<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re looking for a true array_diff_assoc, comparing arrays to determine the difference between two, finding missing values from both, you can use this along with array_merge.<br><br>$a = array(&apos;a&apos;=&gt;1,&apos;b&apos;=&gt;2,&apos;c&apos;=&gt;3);<br>$b = array(&apos;a&apos;=&gt;1,&apos;b&apos;=&gt;2,&apos;d&apos;=&gt;4);<br>print_r(array_diff_assoc($a,$b));<br>// returns:<br>array<br>(<br>&#xA0; &#xA0; [c] =&gt; 3<br>)<br><br>print_r(array_diff_assoc($b,$a));<br>// returns <br>array<br>(<br>&#xA0; &#xA0; [d] =&gt; 4<br>)<br><br>print_r(array_merge(array_diff_assoc($a,$b),array_diff_assoc($b,$a)));<br>// returns<br>array<br>(<br>&#xA0; &#xA0; [c] =&gt; 3<br>&#xA0; &#xA0; [d] =&gt; 4<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
The direction of the arguments does actually make a difference:
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;x&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;x&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;y&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;y&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;z&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;z&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;t&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;t&apos;</span><span class="keyword">,
<br>);
<br>
<br></span><span class="default">$b </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="string">&apos;x&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;x&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;y&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;y&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;z&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;z&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;t&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;t&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="string">&apos;g&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;g&apos;</span><span class="keyword">,
<br>);
<br>
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_diff_assoc</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">));
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">array_diff_assoc</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">, </span><span class="default">$a</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>echoes:
<br>
<br>Array
<br>(
<br>)
<br>Array
<br>(
<br>&#xA0; &#xA0; [g] =&gt; g
<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
The following will recursively do an array_diff_assoc, which will calculate differences on a multi-dimensional level.&#xA0; This not display any notices if a key don&apos;t exist and if error_reporting is set to E_ALL:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">array_diff_assoc_recursive</span><span class="keyword">(</span><span class="default">$array1</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; foreach(</span><span class="default">$array1 </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!isset(</span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif(!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$new_diff </span><span class="keyword">= </span><span class="default">array_diff_assoc_recursive</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">, </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$new_diff </span><span class="keyword">!= </span><span class="default">FALSE</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$new_diff</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; elseif(!isset(</span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]) || </span><span class="default">$array2</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] != </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$difference</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return !isset(</span><span class="default">$difference</span><span class="keyword">) ? </span><span class="default">0 </span><span class="keyword">: </span><span class="default">$difference</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>[NOTE BY danbrown AT php DOT net: This is a combination of efforts from previous notes deleted.&#xA0; Contributors included (Michael Johnson), (jochem AT iamjochem DAWT com), (sc1n AT yahoo DOT com), and (anders DOT carlsson AT mds DOT mdh DOT se).]</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-diff-assoc.php)

**[⬆ to root](/)**