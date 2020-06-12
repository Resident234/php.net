# Arrays




<div class="phpcode"><span class="html">
please note that when arrays are copied, the &quot;reference status&quot; of their members is preserved (<a href="http://www.php.net/manual/en/language.references.whatdo.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/language.references.whatdo.php</a>).</span>
</div>
  

#


<div class="phpcode"><span class="html">
I think your first, main example is needlessly confusing, very confusing to newbies:<br><br>$array = array(<br>&#xA0; &#xA0; &quot;foo&quot; =&gt; &quot;bar&quot;,<br>&#xA0; &#xA0; &quot;bar&quot; =&gt; &quot;foo&quot;,<br>);<br><br>It should be removed.<br> <br>For newbies:<br>An array index can be any string value, even a value that is also a value in the array. <br>The value of array[&quot;foo&quot;] is &quot;bar&quot;.<br>The value of array[&quot;bar&quot;] is &quot;foo&quot;<br><br>The following expressions are both true:<br>$array[&quot;foo&quot;] == &quot;bar&quot;<br>$array[&quot;bar&quot;] == &quot;foo&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware that if you&apos;re using strings as indices in the $_POST array, that periods are transformed into underscores:<br><br>&lt;html&gt;<br>&lt;body&gt;<br><span class="default">&lt;?php<br>&#xA0; &#xA0; printf</span><span class="keyword">(</span><span class="string">&quot;POST: &quot;</span><span class="keyword">); </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">); </span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;&lt;br/&gt;&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>&lt;form method=&quot;post&quot; action=&quot;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;PHP_SELF&apos;</span><span class="keyword">]; </span><span class="default">?&gt;</span>&quot;&gt;<br>&#xA0; &#xA0; &lt;input type=&quot;hidden&quot; name=&quot;Windows3.1&quot; value=&quot;Sux&quot;&gt;<br>&#xA0; &#xA0; &lt;input type=&quot;submit&quot; value=&quot;Click&quot; /&gt;<br>&lt;/form&gt;<br>&lt;/body&gt;<br>&lt;/html&gt;<br><br>Once you click on the button, the page displays the following:<br><br>POST: Array ( [Windows3_1] =&gt; Sux )</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that array value buckets are reference-safe, even through serialization.
<br>
<br><span class="default">&lt;?php
<br>$x</span><span class="keyword">=</span><span class="string">&apos;initial&apos;</span><span class="keyword">;
<br></span><span class="default">$test</span><span class="keyword">=array(</span><span class="string">&apos;A&apos;</span><span class="keyword">=&gt;&amp;</span><span class="default">$x</span><span class="keyword">,</span><span class="string">&apos;B&apos;</span><span class="keyword">=&gt;&amp;</span><span class="default">$x</span><span class="keyword">);
<br></span><span class="default">$test</span><span class="keyword">=</span><span class="default">unserialize</span><span class="keyword">(</span><span class="default">serialize</span><span class="keyword">(</span><span class="default">$test</span><span class="keyword">));
<br></span><span class="default">$test</span><span class="keyword">[</span><span class="string">&apos;A&apos;</span><span class="keyword">]=</span><span class="string">&apos;changed&apos;</span><span class="keyword">;
<br>echo </span><span class="default">$test</span><span class="keyword">[</span><span class="string">&apos;B&apos;</span><span class="keyword">]; </span><span class="comment">// Outputs &quot;changed&quot;
<br></span><span class="default">?&gt;
<br></span>
<br>This can be useful in some cases, for example saving RAM within complex structures.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Since PHP 7.1, the string will not be converted to array automatically.<br><br>Below codes will fail:<br><br>$a=array();<br>$a[&apos;a&apos;]=&apos;&apos;;<br>$a[&apos;a&apos;][&apos;b&apos;]=&apos;&apos;;&#xA0; <br>//Warning: Illegal string offset &apos;b&apos;<br>//Warning: Cannot assign an empty string to a string offset<br><br>You have to change to as below:<br><br>$a=array();<br><br>$a[&apos;a&apos;]=array(); // Declare it is an array first<br>$a[&apos;a&apos;][&apos;b&apos;]=&apos;&apos;;</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;If you convert a NULL value to an array, you get an empty array.&quot;
<br>
<br>This turns out to be a useful property. Say you have a search function that returns an array of values on success or NULL if nothing found.
<br>
<br><span class="default">&lt;?php $values </span><span class="keyword">= </span><span class="default">search</span><span class="keyword">(...); </span><span class="default">?&gt;
<br></span>
<br>Now you want to merge the array with another array. What do we do if $values is NULL? No problem:
<br>
<br><span class="default">&lt;?php $combined </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">((array)</span><span class="default">$values</span><span class="keyword">, </span><span class="default">$other</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>Voila.</span>
</div>
  

#


<div class="phpcode"><span class="html">
//array keys are always integer and string data type and array values are all data type<br>//type casting and overwriting(data type of array key)<br>//----------------------------------------------------<br>$arr = array(<br>1=&gt;&quot;a&quot;,//int(1)<br>&quot;3&quot;=&gt;&quot;b&quot;,//int(3)<br>&quot;08&quot;=&gt;&quot;c&quot;,//string(2)&quot;08&quot;<br>&quot;80&quot;=&gt;&quot;d&quot;,//int(80)<br>&quot;0&quot;=&gt;&quot;e&quot;,//int(0)<br>&quot;Hellow&quot;=&gt;&quot;f&quot;,//string(6)&quot;Hellow&quot;<br>&quot;10Hellow&quot;=&gt;&quot;h&quot;,//string(8)&quot;10Hellow&quot;<br>1.5=&gt;&quot;j&quot;,//int(1.5)<br>&quot;1.5&quot;=&gt;&quot;k&quot;,//string(3)&quot;1.5&quot;<br>0.0=&gt;&quot;l&quot;,//int(0)<br>false=&gt;&quot;m&quot;,//int(false)<br>true=&gt;&quot;n&quot;,//int(true)<br>&quot;true&quot;=&gt;&quot;o&quot;,//string(4)&quot;true&quot;<br>&quot;false&quot;=&gt;&quot;p&quot;,//string(5)&quot;false&quot;<br>null=&gt;&quot;q&quot;,//string(0)&quot;&quot;<br>NULL=&gt;&quot;r&quot;,//string(0)&quot;&quot; note null and NULL are same<br>&quot;NULL&quot;=&gt;&quot;s&quot;,//string(4)&quot;NULL&quot; ,,,In last element of multiline array,comma is better to used.<br>);<br>//check the data type name of key<br>foreach ($arr as $key =&gt; $value) {<br>var_dump($key);<br>echo &quot;&lt;br&gt;&quot;;<br>}<br><br>//NOte :array and object data type in keys are Illegal ofset.......</span>
</div>
  

#


<div class="phpcode"><span class="html">
--- quote ---<br>Note:<br>Both square brackets and curly braces can be used interchangeably for accessing array elements<br>--- quote end ---<br><br>At least for php 5.4 and 5.6; if function returns an array, the curly brackets does not work directly accessing function result, eg. WillReturnArray(){1} . This will give &quot;syntax error, unexpected &apos;{&apos; in...&quot;.<br>Personally I use only square brackets, expect for accessing single char in string. Old habits...</span>
</div>
  

#


<div class="phpcode"><span class="html">
Used to creating arrays like this in Perl?<br><br>@array = (&quot;All&quot;, &quot;A&quot;..&quot;Z&quot;);<br><br>Looks like we need the range() function in PHP:<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(array(</span><span class="string">&apos;All&apos;</span><span class="keyword">), </span><span class="default">range</span><span class="keyword">(</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;Z&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>You don&apos;t need to array_merge if it&apos;s just one range:<br><br><span class="default">&lt;?php<br>$array </span><span class="keyword">= </span><span class="default">range</span><span class="keyword">(</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;Z&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Regarding the previous comment, beware of the fact that reference to the last value of the array remains stored in $value after the foreach:<br><br><span class="default">&lt;?php<br></span><span class="keyword">foreach ( </span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; &amp;</span><span class="default">$value </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$value </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>}<br><br></span><span class="comment">// without next line you can get bad results...<br>//unset( $value );<br><br></span><span class="default">$value </span><span class="keyword">= </span><span class="default">159</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Now the last element of $arr has the value of &apos;159&apos;. If we remove the comment in the unset() line, everything works as expected ($arr has all values of &apos;1&apos;).<br><br>Bad results can also appear in nested foreach loops (the same reason as above).<br><br>So either unset $value after each foreach or better use the longer form:<br><br><span class="default">&lt;?php<br></span><span class="keyword">foreach ( </span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value </span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$arr</span><span class="keyword">[ </span><span class="default">$key </span><span class="keyword">] = </span><span class="default">1</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When creating arrays , if we have an element with the same value as another element from the same array, we would expect PHP instead of creating new zval container to increase the refcount and point the duplicate symbol to the same zval. This is true except for value type integer. <br>Example:<br><br>$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; 23, &apos;too&apos; =&gt; 23 ];<br>xdebug_debug_zval( &apos;arr&apos; );<br><br>Output:<br>arr:<br><br>(refcount=2, is_ref=0)<br>array (size=3)<br>&#xA0; &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)<br>&#xA0; &apos;age&apos; =&gt; (refcount=0, is_ref=0)int 23<br>&#xA0; &apos;too&apos; =&gt; (refcount=0, is_ref=0)int 23<br><br>but :<br>$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; 23, &apos;too&apos; =&gt; &apos;23&apos; ];<br>xdebug_debug_zval( &apos;arr&apos; );<br><br>will produce:<br>arr:<br><br>(refcount=2, is_ref=0)<br>array (size=3)<br>&#xA0; &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)<br>&#xA0; &apos;age&apos; =&gt; (refcount=0, is_ref=0)int 23<br>&#xA0; &apos;too&apos; =&gt; (refcount=1, is_ref=0)string &apos;23&apos; (length=2)<br>or :<br><br>$arr = [&apos;bebe&apos; =&gt; &apos;Bob&apos;, &apos;age&apos; =&gt; [1,2], &apos;too&apos; =&gt; [1,2] ];<br>xdebug_debug_zval( &apos;arr&apos; );<br><br>Output:<br>arr:<br><br>(refcount=2, is_ref=0)<br>array (size=3)<br>&#xA0; &apos;bebe&apos; =&gt; (refcount=1, is_ref=0)string &apos;Bob&apos; (length=3)<br>&#xA0; &apos;age&apos; =&gt; (refcount=2, is_ref=0)<br>&#xA0; &#xA0; array (size=2)<br>&#xA0; &#xA0; &#xA0; 0 =&gt; (refcount=0, is_ref=0)int 1<br>&#xA0; &#xA0; &#xA0; 1 =&gt; (refcount=0, is_ref=0)int 2<br>&#xA0; &apos;too&apos; =&gt; (refcount=2, is_ref=0)<br>&#xA0; &#xA0; array (size=2)<br>&#xA0; &#xA0; &#xA0; 0 =&gt; (refcount=0, is_ref=0)int 1<br>&#xA0; &#xA0; &#xA0; 1 =&gt; (refcount=0, is_ref=0)int 2</span>
</div>
  

#


<div class="phpcode"><span class="html">
[Editor&apos;s note: You can achieve what you&apos;re looking for by referencing $single, rather than copying it by value in your foreach statement. See <a href="http://php.net/foreach" rel="nofollow" target="_blank">http://php.net/foreach</a> for more details.]
<br>
<br>Don&apos;t know if this is known or not, but it did eat some of my time and maybe it won&apos;t eat your time now...
<br>
<br>I tried to add something to a multidimensional array, but that didn&apos;t work at first, look at the code below to see what I mean:
<br>
<br><span class="default">&lt;?php
<br>
<br>$a1 </span><span class="keyword">= array( </span><span class="string">&quot;a&quot; </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">, </span><span class="string">&quot;b&quot; </span><span class="keyword">=&gt; </span><span class="default">1 </span><span class="keyword">);
<br></span><span class="default">$a2 </span><span class="keyword">= array( </span><span class="string">&quot;aa&quot; </span><span class="keyword">=&gt; </span><span class="default">00</span><span class="keyword">, </span><span class="string">&quot;bb&quot; </span><span class="keyword">=&gt; </span><span class="default">11 </span><span class="keyword">);
<br>
<br></span><span class="default">$together </span><span class="keyword">= array( </span><span class="default">$a1</span><span class="keyword">, </span><span class="default">$a2 </span><span class="keyword">);
<br>
<br>foreach( </span><span class="default">$together </span><span class="keyword">as </span><span class="default">$single </span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$single</span><span class="keyword">[ </span><span class="string">&quot;c&quot; </span><span class="keyword">] = </span><span class="default">3 </span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$together </span><span class="keyword">); 
<br></span><span class="comment">/* nothing changed result is:
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; 0
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>&#xA0; &#xA0; [1] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [aa] =&gt; 0
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [bb] =&gt; 11
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>) */
<br>
<br></span><span class="keyword">foreach( </span><span class="default">$together </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value </span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$together</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">][</span><span class="string">&quot;c&quot;</span><span class="keyword">] = </span><span class="default">3 </span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">$together </span><span class="keyword">);
<br>
<br></span><span class="comment">/* now it works, this prints
<br>Array
<br>(
<br>&#xA0; &#xA0; [0] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [a] =&gt; 0
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [b] =&gt; 1
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [c] =&gt; 3
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>&#xA0; &#xA0; [1] =&gt; Array
<br>&#xA0; &#xA0; &#xA0; &#xA0; (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [aa] =&gt; 0
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [bb] =&gt; 11
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [c] =&gt; 3
<br>&#xA0; &#xA0; &#xA0; &#xA0; )
<br>)
<br>*/
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.array.php)

**[To root](/README.md)**