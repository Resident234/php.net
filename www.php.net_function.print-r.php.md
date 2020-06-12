# print_r




<div class="phpcode"><span class="html">
I add this function to the global scope on just about every project I do, it makes reading the output of print_r() in a browser infinitely easier.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">print_r2</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; echo&#xA0; </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>It also makes sense in some cases to add an if statement to only display the output in certain scenarios, such as:<br><br>if(debug==true)<br>if($_SERVER[&apos;REMOTE_ADDR&apos;] == &apos;127.0.0.1&apos;)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is another version that parses the print_r() output. I tried the one posted, but I had difficulties with it. I believe it has a problem with nested arrays. This handles nested arrays without issue as far as I can tell. 
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">print_r_reverse</span><span class="keyword">(</span><span class="default">$in</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$lines </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$in</span><span class="keyword">));
<br>&#xA0; &#xA0; if (</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) != </span><span class="string">&apos;Array&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// bottomed out to something that isn&apos;t an array
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$in</span><span class="keyword">;
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// this is an array, lets parse it
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;/(\s{5,})\(/&quot;</span><span class="keyword">, </span><span class="default">$lines</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$match</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// this is a tested array/recursive call to this function
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // take a set of spaces off the beginning
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$spaces </span><span class="keyword">= </span><span class="default">$match</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$spaces_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$spaces</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lines_total </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$lines_total</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="default">0</span><span class="keyword">, </span><span class="default">$spaces_length</span><span class="keyword">) == </span><span class="default">$spaces</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lines</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">], </span><span class="default">$spaces_length</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">); </span><span class="comment">// Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">); </span><span class="comment">// (
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$lines</span><span class="keyword">); </span><span class="comment">// )
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$in </span><span class="keyword">= </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="default">$lines</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// make sure we only match stuff with 4 preceding spaces (stuff for this array and not a nested one)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="string">&quot;/^\s{4}\[(.+?)\] \=\&gt; /m&quot;</span><span class="keyword">, </span><span class="default">$in</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">, </span><span class="default">PREG_OFFSET_CAPTURE </span><span class="keyword">| </span><span class="default">PREG_SET_ORDER</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pos </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$previous_key </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$in_length </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$in</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// store the following in $pos:
<br>&#xA0; &#xA0; &#xA0; &#xA0; // array with key = key of the parsed array&apos;s item
<br>&#xA0; &#xA0; &#xA0; &#xA0; // value = array(start position in $in, $end position in $in)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$matches </span><span class="keyword">as </span><span class="default">$match</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$key </span><span class="keyword">= </span><span class="default">$match</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">][</span><span class="default">0</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$start </span><span class="keyword">= </span><span class="default">$match</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="default">1</span><span class="keyword">] + </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$match</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="default">0</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$pos</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = array(</span><span class="default">$start</span><span class="keyword">, </span><span class="default">$in_length</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$previous_key </span><span class="keyword">!= </span><span class="string">&apos;&apos;</span><span class="keyword">) </span><span class="default">$pos</span><span class="keyword">[</span><span class="default">$previous_key</span><span class="keyword">][</span><span class="default">1</span><span class="keyword">] = </span><span class="default">$match</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="default">1</span><span class="keyword">] - </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$previous_key </span><span class="keyword">= </span><span class="default">$key</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ret </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$pos </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$where</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// recursively see if the parsed out value is an array too
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ret</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">print_r_reverse</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$in</span><span class="keyword">, </span><span class="default">$where</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$where</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] - </span><span class="default">$where</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]));
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$ret</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.print-r.php)

**[⬆ to root](/)**