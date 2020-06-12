# explode




<div class="phpcode"><span class="html">
Here is my approach to have exploded output with multiple delimiter. <br><br><span class="default">&lt;?php<br><br></span><span class="comment">//$delimiters has to be array<br>//$string has to be array<br><br></span><span class="keyword">function </span><span class="default">multiexplode </span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">) {<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$ready </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">, </span><span class="default">$delimiters</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$launch </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">], </span><span class="default">$ready</span><span class="keyword">);<br>&#xA0; &#xA0; return&#xA0; </span><span class="default">$launch</span><span class="keyword">;<br>}<br><br></span><span class="default">$text </span><span class="keyword">= </span><span class="string">&quot;here is a sample: this text, and this will be exploded. this also | this one too :)&quot;</span><span class="keyword">;<br></span><span class="default">$exploded </span><span class="keyword">= </span><span class="default">multiexplode</span><span class="keyword">(array(</span><span class="string">&quot;,&quot;</span><span class="keyword">,</span><span class="string">&quot;.&quot;</span><span class="keyword">,</span><span class="string">&quot;|&quot;</span><span class="keyword">,</span><span class="string">&quot;:&quot;</span><span class="keyword">),</span><span class="default">$text</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$exploded</span><span class="keyword">);<br><br></span><span class="comment">//And output will be like this:<br>// Array<br>// (<br>//&#xA0; &#xA0; [0] =&gt; here is a sample<br>//&#xA0; &#xA0; [1] =&gt;&#xA0; this text<br>//&#xA0; &#xA0; [2] =&gt;&#xA0; and this will be exploded<br>//&#xA0; &#xA0; [3] =&gt;&#xA0; this also <br>//&#xA0; &#xA0; [4] =&gt;&#xA0; this one too <br>//&#xA0; &#xA0; [5] =&gt; )<br>// )<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beaware splitting empty strings.<br><br><span class="default">&lt;?php<br>$str </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">$res </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;,&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>If you split an empty string, you get back a one-element array with 0 as the key and an empty string for the value.<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; <br>)<br><br>To solve this, just use array_filter() without callback. Quoting manual page &quot;If the callback function is not supplied, array_filter() will remove all the entries of input that are equal to FALSE.&quot;.<br><br><span class="default">&lt;?php<br>$str </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">$res </span><span class="keyword">= </span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;,&quot;</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">));<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Array<br>(<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
The comments to use array_filter() without a callback to remove empty strings from explode&apos;s results miss the fact that array_filter will remove all elements that, to quote the manual,&#xA0; &quot;are equal to FALSE&quot;.<br><br>This includes, in particular, the string &quot;0&quot;, which is NOT an empty string.<br><br>If you really want to filter out empty strings, use the defining feature of the empty string that it is the only string that has a length of 0. So:<br><span class="default">&lt;?php<br>array_filter</span><span class="keyword">(</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;:&apos;</span><span class="keyword">, </span><span class="string">&quot;1:2::3:0:4&quot;</span><span class="keyword">), </span><span class="string">&apos;strlen&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
a simple one line method to explode &amp; trim whitespaces from the exploded elements<br><br>array_map(&apos;trim&apos;,explode(&quot;,&quot;,$str));<br><br>example:<br><br>$str=&quot;one&#xA0; ,two&#xA0; ,&#xA0; &#xA0; &#xA0;&#xA0; three&#xA0; ,&#xA0; four&#xA0; &#xA0; &quot;; <br>print_r(array_map(&apos;trim&apos;,explode(&quot;,&quot;,$str)));<br><br>Output:<br><br>Array ( [0] =&gt; one [1] =&gt; two [2] =&gt; three [3] =&gt; four )</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">// converts pure string into a trimmed keyed array<br></span><span class="keyword">function </span><span class="default">string2KeyedArray</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$delimiter </span><span class="keyword">= </span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$kv </span><span class="keyword">= </span><span class="string">&apos;=&gt;&apos;</span><span class="keyword">) {<br>&#xA0; if (</span><span class="default">$a </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$delimiter</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">)) { </span><span class="comment">// create parts<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$a </span><span class="keyword">as </span><span class="default">$s</span><span class="keyword">) { </span><span class="comment">// each part<br>&#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$s</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$pos </span><span class="keyword">= </span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">, </span><span class="default">$kv</span><span class="keyword">)) { </span><span class="comment">// key/value delimiter<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ka</span><span class="keyword">[</span><span class="default">trim</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">$pos</span><span class="keyword">))] = </span><span class="default">trim</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">, </span><span class="default">$pos </span><span class="keyword">+ </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$kv</span><span class="keyword">)));<br>&#xA0; &#xA0; &#xA0; &#xA0; } else { </span><span class="comment">// key delimiter not found<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$ka</span><span class="keyword">[] = </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$ka</span><span class="keyword">;<br>&#xA0; }<br>} </span><span class="comment">// string2KeyedArray<br><br></span><span class="default">$string </span><span class="keyword">= </span><span class="string">&apos;a=&gt;1, b=&gt;23&#xA0;&#xA0; , $a, c=&gt; 45% , true,d =&gt; ab c &apos;</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">string2KeyedArray</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Array<br>(<br>&#xA0; [a] =&gt; 1<br>&#xA0; [b] =&gt; 23<br>&#xA0; [0] =&gt; $a<br>&#xA0; [c] =&gt; 45%<br>&#xA0; [1] =&gt; true<br>&#xA0; [d] =&gt; ab c<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a function for &quot;multi&quot; exploding a string.<br><br><span class="default">&lt;?php<br></span><span class="comment">//the function<br>//Param 1 has to be an Array<br>//Param 2 has to be a String<br></span><span class="keyword">function </span><span class="default">multiexplode </span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$ary </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">],</span><span class="default">$string</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">);<br>&#xA0; &#xA0; if(</span><span class="default">$delimiters </span><span class="keyword">!= </span><span class="default">NULL</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$ary </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$ary</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">multiexplode</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">, </span><span class="default">$val</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return&#xA0; </span><span class="default">$ary</span><span class="keyword">;<br>}<br><br></span><span class="comment">// Example of use<br></span><span class="default">$string </span><span class="keyword">= </span><span class="string">&quot;1-2-3|4-5|6:7-8-9-0|1,2:3-4|5&quot;</span><span class="keyword">;<br></span><span class="default">$delimiters </span><span class="keyword">= Array(</span><span class="string">&quot;,&quot;</span><span class="keyword">,</span><span class="string">&quot;:&quot;</span><span class="keyword">,</span><span class="string">&quot;|&quot;</span><span class="keyword">,</span><span class="string">&quot;-&quot;</span><span class="keyword">);<br><br></span><span class="default">$res </span><span class="keyword">= </span><span class="default">multiexplode</span><span class="keyword">(</span><span class="default">$delimiters</span><span class="keyword">,</span><span class="default">$string</span><span class="keyword">);<br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">);<br>echo </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;<br><br></span><span class="comment">//returns<br>/*<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [2] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 5<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [2] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 6<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 7<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 8<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [2] =&gt; 9<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [3] =&gt; 0<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; 4<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; 5<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>)<br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Explode does not parse a string by delimiters, in the sense that we expect to find tokens between a starting and ending delimiter, but instead splits a string into parts by using a string as the boundary of each part. Once that boundary is discovered the string is split. Whether or not that boundary is proceeded or superseded by any data is irrelevant since the parts are determined at the point a boundary is discovered. 
<br>
<br>For example: 
<br>
<br><span class="default">&lt;?php 
<br>
<br>var_dump</span><span class="keyword">(</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;/&quot;</span><span class="keyword">,</span><span class="string">&quot;/&quot;</span><span class="keyword">)); 
<br>
<br></span><span class="comment">/* 
<br>&#xA0;&#xA0; Outputs 
<br>
<br>&#xA0;&#xA0; array(2) { 
<br>&#xA0; &#xA0;&#xA0; [0]=&gt; 
<br>&#xA0; &#xA0;&#xA0; string(0) &quot;&quot; 
<br>&#xA0; &#xA0;&#xA0; [1]=&gt; 
<br>&#xA0; &#xA0;&#xA0; string(0) &quot;&quot; 
<br>&#xA0;&#xA0; } 
<br>*/ 
<br>
<br></span><span class="default">?&gt;</span> 
<br>
<br>The reason we have two empty strings here is that a boundary is discovered before any data has been collected from the string. The boundary splits the string into two parts even though those parts are empty. 
<br>
<br>One way to avoid getting back empty parts (if you don&apos;t care for those empty parts) is to use array_filter on the result. 
<br>
<br><span class="default">&lt;?php 
<br>
<br>var_dump</span><span class="keyword">(</span><span class="default">array_filter</span><span class="keyword">(</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;/&quot;</span><span class="keyword">,</span><span class="string">&quot;/&quot;</span><span class="keyword">))); 
<br>
<br></span><span class="comment">/* 
<br>&#xA0;&#xA0; Outputs 
<br>
<br>&#xA0;&#xA0; array(0) { 
<br>&#xA0;&#xA0; } 
<br>*/ 
<br></span><span class="default">?&gt;</span> 
<br>
<br>*[This note was edited by googleguy at php dot net for clarity]*</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.explode.php)

**[⬆ to root](/)**