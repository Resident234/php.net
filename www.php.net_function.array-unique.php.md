# array_unique




<div class="phpcode"><span class="html">
Create multidimensional array unique for any single key index.
<br>e.g I want to create multi dimentional unique array for specific code
<br>
<br>Code : 
<br>My array is like this,
<br>
<br><span class="default">&lt;?php
<br>$details </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="default">0 </span><span class="keyword">=&gt; array(</span><span class="string">&quot;id&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;1&quot;</span><span class="keyword">, </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Mike&quot;</span><span class="keyword">,&#xA0; &#xA0; </span><span class="string">&quot;num&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;9876543210&quot;</span><span class="keyword">),
<br>&#xA0; &#xA0; </span><span class="default">1 </span><span class="keyword">=&gt; array(</span><span class="string">&quot;id&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;2&quot;</span><span class="keyword">, </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Carissa&quot;</span><span class="keyword">, </span><span class="string">&quot;num&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;08548596258&quot;</span><span class="keyword">),
<br>&#xA0; &#xA0; </span><span class="default">2 </span><span class="keyword">=&gt; array(</span><span class="string">&quot;id&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;1&quot;</span><span class="keyword">, </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Mathew&quot;</span><span class="keyword">,&#xA0; </span><span class="string">&quot;num&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;784581254&quot;</span><span class="keyword">),
<br>);
<br></span><span class="default">?&gt;
<br></span>
<br>You can make it unique for any field like id, name or num.
<br>
<br>I have develop this function for same : 
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">unique_multidim_array</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">$key</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$temp_array </span><span class="keyword">= array();
<br>&#xA0; &#xA0; </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$key_array </span><span class="keyword">= array();
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; foreach(</span><span class="default">$array </span><span class="keyword">as </span><span class="default">$val</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">], </span><span class="default">$key_array</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$key_array</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$temp_array</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$temp_array</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Now, call this function anywhere from your code,
<br>
<br>something like this,
<br><span class="default">&lt;?php
<br>$details </span><span class="keyword">= </span><span class="default">unique_multidim_array</span><span class="keyword">(</span><span class="default">$details</span><span class="keyword">,</span><span class="string">&apos;id&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Output will be like this :
<br><span class="default">&lt;?php
<br>$details </span><span class="keyword">= array(
<br>&#xA0; &#xA0; </span><span class="default">0 </span><span class="keyword">=&gt; array(</span><span class="string">&quot;id&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;1&quot;</span><span class="keyword">,</span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Mike&quot;</span><span class="keyword">,</span><span class="string">&quot;num&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;9876543210&quot;</span><span class="keyword">),
<br>&#xA0; &#xA0; </span><span class="default">1 </span><span class="keyword">=&gt; array(</span><span class="string">&quot;id&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;2&quot;</span><span class="keyword">,</span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;Carissa&quot;</span><span class="keyword">,</span><span class="string">&quot;num&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;08548596258&quot;</span><span class="keyword">),
<br>);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s often faster to use a foreache and array_keys than array_unique:
<br>
<br>&#xA0; &#xA0; <span class="default">&lt;?php
<br>
<br>&#xA0; &#xA0; $max </span><span class="keyword">= </span><span class="default">1000000</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$arr </span><span class="keyword">= </span><span class="default">range</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">$max</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$arr2 </span><span class="keyword">= </span><span class="default">range</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">$max</span><span class="keyword">,</span><span class="default">2</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$arr </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">,</span><span class="default">$arr2</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$time </span><span class="keyword">= -</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$res1 </span><span class="keyword">= </span><span class="default">array_unique</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$time </span><span class="keyword">+= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;deduped to &quot;</span><span class="keyword">.</span><span class="default">count</span><span class="keyword">(</span><span class="default">$res1</span><span class="keyword">).</span><span class="string">&quot; in &quot;</span><span class="keyword">.</span><span class="default">$time</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// deduped to 666667 in 32.300781965256
<br>
<br>&#xA0; &#xA0; </span><span class="default">$time </span><span class="keyword">= -</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$res2 </span><span class="keyword">= array();
<br>&#xA0; &#xA0; foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key</span><span class="keyword">=&gt;</span><span class="default">$val</span><span class="keyword">) {&#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$res2</span><span class="keyword">[</span><span class="default">$val</span><span class="keyword">] = </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">$res2 </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$res2</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$time </span><span class="keyword">+= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;br /&gt;deduped to &quot;</span><span class="keyword">.</span><span class="default">count</span><span class="keyword">(</span><span class="default">$res2</span><span class="keyword">).</span><span class="string">&quot; in &quot;</span><span class="keyword">.</span><span class="default">$time</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// deduped to 666667 in 0.84372591972351
<br>
<br>&#xA0; &#xA0; </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For people looking at the flip flip method for getting unique values in a simple array. This is the absolute fastest method:
<br>
<br><span class="default">&lt;?php
<br>$unique </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>It&apos;s marginally faster as:
<br><span class="default">&lt;?php
<br>$unique </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)));
<br></span><span class="default">?&gt;
<br></span>
<br>And it&apos;s marginally slower as:
<br><span class="default">&lt;?php
<br>$unique array_flip</span><span class="keyword">(</span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)); </span><span class="comment">// leaves gaps
<br></span><span class="default">?&gt;
<br></span>
<br>It&apos;s still about twice as fast or fast as array_unique.
<br>
<br>This tested on several different machines with 100000 random arrays. All machines used a version of PHP5.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I needed to identify email addresses in a data table that were replicated, so I wrote the array_not_unique() function:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">array_not_unique</span><span class="keyword">(</span><span class="default">$raw_array</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$dupes </span><span class="keyword">= array();<br>&#xA0; &#xA0; </span><span class="default">natcasesort</span><span class="keyword">(</span><span class="default">$raw_array</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">reset </span><span class="keyword">(</span><span class="default">$raw_array</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="default">$old_key&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">NULL</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$old_value&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">NULL</span><span class="keyword">;<br>&#xA0; &#xA0; foreach (</span><span class="default">$raw_array </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$value </span><span class="keyword">=== </span><span class="default">NULL</span><span class="keyword">) { continue; }<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$old_value </span><span class="keyword">== </span><span class="default">$value</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dupes</span><span class="keyword">[</span><span class="default">$old_key</span><span class="keyword">]&#xA0; &#xA0; = </span><span class="default">$old_value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dupes</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]&#xA0; &#xA0; &#xA0; &#xA0; = </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$old_value&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">$value</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$old_key&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">$key</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>return </span><span class="default">$dupes</span><span class="keyword">;<br>}<br><br></span><span class="default">$raw_array&#xA0; &#xA0;&#xA0; </span><span class="keyword">= array();<br></span><span class="default">$raw_array</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]&#xA0; &#xA0; = </span><span class="string">&apos;abc@xyz.com&apos;</span><span class="keyword">;<br></span><span class="default">$raw_array</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]&#xA0; &#xA0; = </span><span class="string">&apos;def@xyz.com&apos;</span><span class="keyword">;<br></span><span class="default">$raw_array</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]&#xA0; &#xA0; = </span><span class="string">&apos;ghi@xyz.com&apos;</span><span class="keyword">;<br></span><span class="default">$raw_array</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">]&#xA0; &#xA0; = </span><span class="string">&apos;abc@xyz.com&apos;</span><span class="keyword">; </span><span class="comment">// Duplicate<br><br></span><span class="default">$common_stuff&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">array_not_unique</span><span class="keyword">(</span><span class="default">$raw_array</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$common_stuff</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Case insensitive; will keep first encountered value.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">function </span><span class="default">array_iunique</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$lowered </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;strtolower&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">array_intersect_key</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">, </span><span class="default">array_unique</span><span class="keyword">(</span><span class="default">$lowered</span><span class="keyword">));
<br>}
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
recursive array unique for multiarrays<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">super_unique</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">)<br>{<br>&#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&quot;unserialize&quot;</span><span class="keyword">, </span><span class="default">array_unique</span><span class="keyword">(</span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&quot;serialize&quot;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">)));<br><br>&#xA0; foreach (</span><span class="default">$result </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; if ( </span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">) )<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">super_unique</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; }<br><br>&#xA0; return </span><span class="default">$result</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-unique.php)

**[⬆ to root](/)**