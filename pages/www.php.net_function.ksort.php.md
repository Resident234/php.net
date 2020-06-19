# ksort




<div class="phpcode"><span class="html">
A nice way to do sorting of a key on a multi-dimensional array without having to know what keys you have in the array first:
<br>
<br><span class="default">&lt;?php
<br>$people </span><span class="keyword">= array(
<br>array(</span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Bob&quot;</span><span class="keyword">,</span><span class="string">&quot;age&quot;</span><span class="keyword">=&gt;</span><span class="default">8</span><span class="keyword">,</span><span class="string">&quot;colour&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;red&quot;</span><span class="keyword">),
<br>array(</span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Greg&quot;</span><span class="keyword">,</span><span class="string">&quot;age&quot;</span><span class="keyword">=&gt;</span><span class="default">12</span><span class="keyword">,</span><span class="string">&quot;colour&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;blue&quot;</span><span class="keyword">),
<br>array(</span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Andy&quot;</span><span class="keyword">,</span><span class="string">&quot;age&quot;</span><span class="keyword">=&gt;</span><span class="default">5</span><span class="keyword">,</span><span class="string">&quot;colour&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;purple&quot;</span><span class="keyword">));
<br>
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$people</span><span class="keyword">);
<br>
<br></span><span class="default">$sortArray </span><span class="keyword">= array();
<br>
<br>foreach(</span><span class="default">$people </span><span class="keyword">as </span><span class="default">$person</span><span class="keyword">){
<br>&#xA0; &#xA0; foreach(</span><span class="default">$person </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!isset(</span><span class="default">$sortArray</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">])){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$sortArray</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$sortArray</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">][] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">$orderby </span><span class="keyword">= </span><span class="string">&quot;name&quot;</span><span class="keyword">; </span><span class="comment">//change this to whatever key you want from the array
<br>
<br></span><span class="default">array_multisort</span><span class="keyword">(</span><span class="default">$sortArray</span><span class="keyword">[</span><span class="default">$orderby</span><span class="keyword">],</span><span class="default">SORT_DESC</span><span class="keyword">,</span><span class="default">$people</span><span class="keyword">);
<br>
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$people</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Output from first var_dump:
<br>
<br>[0]=&amp;gt;
<br>&#xA0; array(3) {
<br>&#xA0; &#xA0; [&quot;name&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(3) &quot;Bob&quot;
<br>&#xA0; &#xA0; [&quot;age&quot;]=&amp;gt;
<br>&#xA0; &#xA0; int(8)
<br>&#xA0; &#xA0; [&quot;colour&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(3) &quot;red&quot;
<br>&#xA0; }
<br>&#xA0; [1]=&amp;gt;
<br>&#xA0; array(3) {
<br>&#xA0; &#xA0; [&quot;name&quot;]=&amp;gt;
<br>
<br>&#xA0; &#xA0; string(4) &quot;Greg&quot;
<br>&#xA0; &#xA0; [&quot;age&quot;]=&amp;gt;
<br>&#xA0; &#xA0; int(12)
<br>&#xA0; &#xA0; [&quot;colour&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(4) &quot;blue&quot;
<br>&#xA0; }
<br>&#xA0; [2]=&amp;gt;
<br>&#xA0; array(3) {
<br>&#xA0; &#xA0; [&quot;name&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(4) &quot;Andy&quot;
<br>&#xA0; &#xA0; [&quot;age&quot;]=&amp;gt;
<br>&#xA0; &#xA0; int(5)
<br>&#xA0; &#xA0; [&quot;colour&quot;]=&amp;gt;
<br>
<br>&#xA0; &#xA0; string(6) &quot;purple&quot;
<br>&#xA0; }
<br>}
<br>
<br>Output from 2nd var_dump:
<br>
<br>array(3) {
<br>&#xA0; [0]=&amp;gt;
<br>&#xA0; array(3) {
<br>&#xA0; &#xA0; [&quot;name&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(4) &quot;Greg&quot;
<br>&#xA0; &#xA0; [&quot;age&quot;]=&amp;gt;
<br>&#xA0; &#xA0; int(12)
<br>&#xA0; &#xA0; [&quot;colour&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(4) &quot;blue&quot;
<br>&#xA0; }
<br>&#xA0; [1]=&amp;gt;
<br>&#xA0; array(3) {
<br>&#xA0; &#xA0; [&quot;name&quot;]=&amp;gt;
<br>
<br>&#xA0; &#xA0; string(3) &quot;Bob&quot;
<br>&#xA0; &#xA0; [&quot;age&quot;]=&amp;gt;
<br>&#xA0; &#xA0; int(8)
<br>&#xA0; &#xA0; [&quot;colour&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(3) &quot;red&quot;
<br>&#xA0; }
<br>&#xA0; [2]=&amp;gt;
<br>&#xA0; array(3) {
<br>&#xA0; &#xA0; [&quot;name&quot;]=&amp;gt;
<br>&#xA0; &#xA0; string(4) &quot;Andy&quot;
<br>&#xA0; &#xA0; [&quot;age&quot;]=&amp;gt;
<br>&#xA0; &#xA0; int(5)
<br>&#xA0; &#xA0; [&quot;colour&quot;]=&amp;gt;
<br>
<br>&#xA0; &#xA0; string(6) &quot;purple&quot;
<br>&#xA0; }
<br>
<br>There&apos;s no checking on whether your array keys exist, or the array data you are searching on is actually there, but easy enough to add.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I wrote this function to sort the keys of an array using an array of keynames, in order.<br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * function array_reorder_keys<br> * reorder the keys of an array in order of specified keynames; all other nodes not in $keynames will come after last $keyname, in normal array order<br> * @param array &amp;$array - the array to reorder<br> * @param mixed $keynames - a csv or array of keynames, in the order that keys should be reordered<br> */<br></span><span class="keyword">function </span><span class="default">array_reorder_keys</span><span class="keyword">(&amp;</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$keynames</span><span class="keyword">){<br>&#xA0; &#xA0; if(empty(</span><span class="default">$array</span><span class="keyword">) || !</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) || empty(</span><span class="default">$keynames</span><span class="keyword">)) return;<br>&#xA0; &#xA0; if(!</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$keynames</span><span class="keyword">)) </span><span class="default">$keynames </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">,</span><span class="default">$keynames</span><span class="keyword">);<br>&#xA0; &#xA0; if(!empty(</span><span class="default">$keynames</span><span class="keyword">)) </span><span class="default">$keynames </span><span class="keyword">= </span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">$keynames</span><span class="keyword">);<br>&#xA0; &#xA0; foreach(</span><span class="default">$keynames </span><span class="keyword">as </span><span class="default">$n</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">)){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newarray </span><span class="keyword">= array(</span><span class="default">$n</span><span class="keyword">=&gt;</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$n</span><span class="keyword">]); </span><span class="comment">//copy the node before unsetting<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">unset(</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$n</span><span class="keyword">]); </span><span class="comment">//remove the node<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= </span><span class="default">$newarray </span><span class="keyword">+ </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">); </span><span class="comment">//combine copy with filtered array<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">$seed_array </span><span class="keyword">= array(</span><span class="string">&apos;foo&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;bar&apos;</span><span class="keyword">, </span><span class="string">&apos;someotherkey&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;whatev&apos;</span><span class="keyword">, </span><span class="string">&apos;bar&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;baz&apos;</span><span class="keyword">, </span><span class="string">&apos;baz&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;foo&apos;</span><span class="keyword">, </span><span class="string">&apos;anotherkey&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;anotherval&apos;</span><span class="keyword">);<br></span><span class="default">array_reorder_keys</span><span class="keyword">(</span><span class="default">$seed_array</span><span class="keyword">, </span><span class="string">&apos;baz,foo,bar&apos;</span><span class="keyword">); </span><span class="comment">//returns array(&apos;baz&apos;=&gt;&apos;foo&apos;, &apos;foo&apos;=&gt;&apos;bar&apos;, &apos;bar&apos;=&gt;&apos;baz&apos;, &apos;someotherkey&apos;=&gt;&apos;whatev&apos;, &apos;anotherkey&apos;=&gt;&apos;anotherval&apos; );<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a function to sort an array by the key of his sub-array.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">sksort</span><span class="keyword">(&amp;</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$subkey</span><span class="keyword">=</span><span class="string">&quot;id&quot;</span><span class="keyword">, </span><span class="default">$sort_ascending</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">) {<br><br>&#xA0; &#xA0; if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$temp_array</span><span class="keyword">[</span><span class="default">key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)] = </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br><br>&#xA0; &#xA0; foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$offset </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$found </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$temp_array </span><span class="keyword">as </span><span class="default">$tmp_key </span><span class="keyword">=&gt; </span><span class="default">$tmp_val</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$found </span><span class="keyword">and </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">[</span><span class="default">$subkey</span><span class="keyword">]) &gt; </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$tmp_val</span><span class="keyword">[</span><span class="default">$subkey</span><span class="keyword">]))<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$temp_array </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(&#xA0; &#xA0; (array)</span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$temp_array</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">$offset</span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array(</span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$temp_array</span><span class="keyword">,</span><span class="default">$offset</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$found </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$offset</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$found</span><span class="keyword">) </span><span class="default">$temp_array </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$temp_array</span><span class="keyword">, array(</span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">));<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; if (</span><span class="default">$sort_ascending</span><span class="keyword">) </span><span class="default">$array </span><span class="keyword">= </span><span class="default">array_reverse</span><span class="keyword">(</span><span class="default">$temp_array</span><span class="keyword">);<br><br>&#xA0; &#xA0; else </span><span class="default">$array </span><span class="keyword">= </span><span class="default">$temp_array</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>Example<br><span class="default">&lt;?php<br>$info </span><span class="keyword">= array(</span><span class="string">&quot;peter&quot; </span><span class="keyword">=&gt; array(</span><span class="string">&quot;age&quot; </span><span class="keyword">=&gt; </span><span class="default">21</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;gender&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;male&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;john&quot;&#xA0; </span><span class="keyword">=&gt; array(</span><span class="string">&quot;age&quot; </span><span class="keyword">=&gt; </span><span class="default">19</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;gender&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;male&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">),<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;mary&quot; </span><span class="keyword">=&gt; array(</span><span class="string">&quot;age&quot; </span><span class="keyword">=&gt; </span><span class="default">20</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&quot;gender&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;female&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; );<br><br></span><span class="default">sksort</span><span class="keyword">(</span><span class="default">$info</span><span class="keyword">, </span><span class="string">&quot;age&quot;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$info</span><span class="keyword">);<br><br></span><span class="default">sksort</span><span class="keyword">(</span><span class="default">$info</span><span class="keyword">, </span><span class="string">&quot;age&quot;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$ifno</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This will be the output of the example:<br><br>/*DESCENDING SORT*/<br>array(3) {<br>&#xA0; [&quot;peter&quot;]=&gt;<br>&#xA0; array(2) {<br>&#xA0; &#xA0; [&quot;age&quot;]=&gt;<br>&#xA0; &#xA0; int(21)<br>&#xA0; &#xA0; [&quot;gender&quot;]=&gt;<br>&#xA0; &#xA0; string(4) &quot;male&quot;<br>&#xA0; }<br>&#xA0; [&quot;mary&quot;]=&gt;<br>&#xA0; array(2) {<br>&#xA0; &#xA0; [&quot;age&quot;]=&gt;<br>&#xA0; &#xA0; int(20)<br>&#xA0; &#xA0; [&quot;gender&quot;]=&gt;<br>&#xA0; &#xA0; string(6) &quot;female&quot;<br>&#xA0; }<br>&#xA0; [&quot;john&quot;]=&gt;<br>&#xA0; array(2) {<br>&#xA0; &#xA0; [&quot;age&quot;]=&gt;<br>&#xA0; &#xA0; int(19)<br>&#xA0; &#xA0; [&quot;gender&quot;]=&gt;<br>&#xA0; &#xA0; string(4) &quot;male&quot;<br>&#xA0; }<br>}<br><br>/*ASCENDING SORT*/<br>array(3) {<br>&#xA0; [&quot;john&quot;]=&gt;<br>&#xA0; array(2) {<br>&#xA0; &#xA0; [&quot;age&quot;]=&gt;<br>&#xA0; &#xA0; int(19)<br>&#xA0; &#xA0; [&quot;gender&quot;]=&gt;<br>&#xA0; &#xA0; string(4) &quot;male&quot;<br>&#xA0; }<br>&#xA0; [&quot;mary&quot;]=&gt;<br>&#xA0; array(2) {<br>&#xA0; &#xA0; [&quot;age&quot;]=&gt;<br>&#xA0; &#xA0; int(20)<br>&#xA0; &#xA0; [&quot;gender&quot;]=&gt;<br>&#xA0; &#xA0; string(6) &quot;female&quot;<br>&#xA0; }<br>&#xA0; [&quot;peter&quot;]=&gt;<br>&#xA0; array(2) {<br>&#xA0; &#xA0; [&quot;age&quot;]=&gt;<br>&#xA0; &#xA0; int(21)<br>&#xA0; &#xA0; [&quot;gender&quot;]=&gt;<br>&#xA0; &#xA0; string(4) &quot;male&quot;<br>&#xA0; }<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ksort.php)

**[To root](/README.md)**