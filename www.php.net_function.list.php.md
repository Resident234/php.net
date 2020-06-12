# list




<div class="phpcode"><span class="html">
Since PHP 7.1, keys can be specified<br><br>exemple : <br><span class="default">&lt;?php <br>$array </span><span class="keyword">= [</span><span class="string">&apos;locality&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Tunis&apos;</span><span class="keyword">, </span><span class="string">&apos;postal_code&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;1110&apos;</span><span class="keyword">];<br><br>list(</span><span class="string">&apos;postal_code&apos; </span><span class="keyword">=&gt; </span><span class="default">$zipCode</span><span class="keyword">, </span><span class="string">&apos;locality&apos; </span><span class="keyword">=&gt; </span><span class="default">$locality</span><span class="keyword">) = </span><span class="default">$array</span><span class="keyword">;<br><br>print </span><span class="default">$zipCode</span><span class="keyword">; </span><span class="comment">// will output 1110<br></span><span class="keyword">print </span><span class="default">$locality</span><span class="keyword">; </span><span class="comment">// will output Tunis<br> </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In PHP 7.1 we can do the following:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">[</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">, </span><span class="default">$c</span><span class="keyword">] = [</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">, </span><span class="string">&apos;c&apos;</span><span class="keyword">];<br></span><span class="default">?&gt;<br></span><br>Before, we had to do:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">list(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">, </span><span class="default">$c</span><span class="keyword">) = [</span><span class="string">&apos;a&apos;</span><span class="keyword">, </span><span class="string">&apos;b&apos;</span><span class="keyword">,&#xA0; </span><span class="string">&apos;c&apos;</span><span class="keyword">];<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The example showing that:<br><br>$info = array(&apos;kawa&apos;, &apos;br&#x105;zowa&apos;, &apos;kofeina&apos;);<br>list($a[0], $a[1], $a[2]) = $info;<br>var_dump($a);<br><br>outputs:<br>array(3) {<br>[2]=&gt;<br>string(8) &quot;kofeina&quot;<br>[1]=&gt;<br>string(5) &quot;br&#x105;zowa&quot;<br>[0]=&gt;<br>string(6) &quot;kawa&quot;<br>}<br><br>One thing to note here is that if you define the array earlier, e.g.:<br>$a = [0, 0, 0];<br><br>the indexes will be kept in the correct order:<br><br>array(3) {<br>&#xA0; [0]=&gt;<br>&#xA0; string(4) &quot;kawa&quot;<br>&#xA0; [1]=&gt;<br>&#xA0; string(8) &quot;br&#x105;zowa&quot;<br>&#xA0; [2]=&gt;<br>&#xA0; string(7) &quot;kofeina&quot;<br>}<br><br>Thought that it was worth mentioning.</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">/**<br> * It seems you can skip listed values.<br> * Here&apos;s an example to show what I mean.<br> *<br> * FYI works just as well with PHP 7.1 shorthand list syntax.<br> * Tested against PHP 5.6.30, 7.1.5<br> */<br></span><span class="default">$a </span><span class="keyword">= [ </span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">, </span><span class="default">4 </span><span class="keyword">];<br><br></span><span class="comment">// this is quite normal use case for list<br></span><span class="keyword">echo </span><span class="string">&quot;Unpack all values\n&quot;</span><span class="keyword">;<br>list(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">) = </span><span class="default">$a</span><span class="keyword">;<br>echo </span><span class="string">&quot;</span><span class="default">$v1</span><span class="string">, </span><span class="default">$v2</span><span class="string">, </span><span class="default">$v3</span><span class="string">, </span><span class="default">$v4</span><span class="string">\n&quot;</span><span class="keyword">;<br>unset(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">);<br><br></span><span class="comment">// this is what I mean:<br></span><span class="keyword">echo </span><span class="string">&quot;Skip middle\n&quot;</span><span class="keyword">;<br>list(</span><span class="default">$v1</span><span class="keyword">, , , </span><span class="default">$v4</span><span class="keyword">) = </span><span class="default">$a</span><span class="keyword">;<br>echo </span><span class="string">&quot;</span><span class="default">$v1</span><span class="string">, </span><span class="default">$v2</span><span class="string">, </span><span class="default">$v3</span><span class="string">, </span><span class="default">$v4</span><span class="string">\n&quot;</span><span class="keyword">;<br>unset(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">);<br><br>echo </span><span class="string">&quot;Skip beginning\n&quot;</span><span class="keyword">;<br>list( , , </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">) = </span><span class="default">$a</span><span class="keyword">;<br>echo </span><span class="string">&quot;</span><span class="default">$v1</span><span class="string">, </span><span class="default">$v2</span><span class="string">, </span><span class="default">$v3</span><span class="string">, </span><span class="default">$v4</span><span class="string">\n&quot;</span><span class="keyword">;<br>unset(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">);<br><br>echo </span><span class="string">&quot;Skip end\n&quot;</span><span class="keyword">;<br>list(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, , ) = </span><span class="default">$a</span><span class="keyword">;<br>echo </span><span class="string">&quot;</span><span class="default">$v1</span><span class="string">, </span><span class="default">$v2</span><span class="string">, </span><span class="default">$v3</span><span class="string">, </span><span class="default">$v4</span><span class="string">\n&quot;</span><span class="keyword">;<br>unset(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">);<br><br>echo </span><span class="string">&quot;Leave middle\n&quot;</span><span class="keyword">;<br>list( , </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, ) = </span><span class="default">$a</span><span class="keyword">;<br>echo </span><span class="string">&quot;</span><span class="default">$v1</span><span class="string">, </span><span class="default">$v2</span><span class="string">, </span><span class="default">$v3</span><span class="string">, </span><span class="default">$v4</span><span class="string">\n&quot;</span><span class="keyword">;<br>unset(</span><span class="default">$v1</span><span class="keyword">, </span><span class="default">$v2</span><span class="keyword">, </span><span class="default">$v3</span><span class="keyword">, </span><span class="default">$v4</span><span class="keyword">);</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
As noted, list() will give an error if the input array is too short. This can be avoided by array_merge()&apos;ing in some default values. For example:<br><br><span class="default">&lt;?php<br>$parameter </span><span class="keyword">= </span><span class="string">&apos;name&apos;</span><span class="keyword">;<br>list( </span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b </span><span class="keyword">) = </span><span class="default">array_merge</span><span class="keyword">( </span><span class="default">explode</span><span class="keyword">( </span><span class="string">&apos;=&apos;</span><span class="keyword">, </span><span class="default">$parameter </span><span class="keyword">), array( </span><span class="default">true </span><span class="keyword">) );<br></span><span class="default">?&gt;<br></span><br>However, you will have to array_merge with an array long enough to ensure there are enough elements (if $parameter is empty, the code above would still error).<br><br>An alternate approach would be to use array_pad on the array to ensure its length (if all the defaults you need to add are the same).<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $parameter </span><span class="keyword">= </span><span class="string">&apos;bob-12345&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; list( </span><span class="default">$name</span><span class="keyword">, </span><span class="default">$id</span><span class="keyword">, </span><span class="default">$fav_color</span><span class="keyword">, </span><span class="default">$age </span><span class="keyword">) = </span><span class="default">array_pad</span><span class="keyword">( </span><span class="default">explode</span><span class="keyword">( </span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="default">$parameter </span><span class="keyword">), </span><span class="default">4</span><span class="keyword">, </span><span class="string">&apos;&apos; </span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="default">$id</span><span class="keyword">, </span><span class="default">$fav_color</span><span class="keyword">, </span><span class="default">$age</span><span class="keyword">);<br></span><span class="comment">/* outputs<br>string(3) &quot;bob&quot;<br>string(5) &quot;12345&quot;<br>string(0) &quot;&quot;<br>string(0) &quot;&quot;<br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The example states the following:<br><span class="default">&lt;?php<br></span><span class="comment">// list() doesn&apos;t work with strings<br></span><span class="keyword">list(</span><span class="default">$bar</span><span class="keyword">) = </span><span class="string">&quot;abcde&quot;</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$bar</span><span class="keyword">); <br></span><span class="comment">// output: NULL<br></span><span class="default">?&gt;<br></span><br>If the string is in a variable however, it seems using list() will treat the string as an array:<br><span class="default">&lt;?php<br>$string </span><span class="keyword">= </span><span class="string">&quot;abcde&quot;</span><span class="keyword">;<br>list(</span><span class="default">$foo</span><span class="keyword">) = </span><span class="default">$string</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">);<br></span><span class="comment">// output: string(1) &quot;a&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
list() can be used with foreach<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= [[</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">], [</span><span class="default">3</span><span class="keyword">, </span><span class="default">4</span><span class="keyword">], [</span><span class="default">5</span><span class="keyword">, </span><span class="default">6</span><span class="keyword">]];<br><br>foreach(</span><span class="default">$array </span><span class="keyword">as list(</span><span class="default">$odd</span><span class="keyword">, </span><span class="default">$even</span><span class="keyword">)){<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$odd</span><span class="string"> is odd; </span><span class="default">$even</span><span class="string"> is even&quot;</span><span class="keyword">, </span><span class="default">PHP_EOL</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>The output:<br>===<br>1 is odd; 2 is even<br>3 is odd; 4 is even<br>5 is odd; 6 is even</span>
</div>
  

#


<div class="phpcode"><span class="html">
The list() definition won&apos;t throw an error if your array is longer then defined list. <br><span class="default">&lt;?php<br><br></span><span class="keyword">list(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">, </span><span class="default">$c</span><span class="keyword">) = array(</span><span class="string">&quot;a&quot;</span><span class="keyword">, </span><span class="string">&quot;b&quot;</span><span class="keyword">, </span><span class="string">&quot;c&quot;</span><span class="keyword">, </span><span class="string">&quot;d&quot;</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">); </span><span class="comment">// a<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">); </span><span class="comment">// b<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$c</span><span class="keyword">); </span><span class="comment">// c<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The list construct seems to look for a sequential list of indexes rather taking elements in sequence. What that obscure statement means is that if you unset an element, list will not simply jump to the next element and assign that to the variable but will treat the missing element as a null or empty variable:<br><br>&#xA0; &#xA0; $test = array(&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;);<br>&#xA0; &#xA0; unset($test[1]);<br>&#xA0; &#xA0; list($a,$b,$c)=$test;<br>&#xA0; &#xA0; print &quot;\$a=&apos;$a&apos; \$b=&apos;$b&apos; \$c=&apos;$c&apos;&lt;BR&gt;&quot;;<br><br>results in:<br>$a=&apos;a&apos; $b=&apos;&apos; $c=&apos;c&apos;<br><br>not:<br>$a=&apos;a&apos; $b=&apos;c&apos; $c=&apos;d&apos;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.list.php)

**[⬆ to root](/)**