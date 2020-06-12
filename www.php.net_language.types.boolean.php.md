# Booleans




<div class="phpcode"><span class="html">
Ah, yes, booleans - bit values that are either set (TRUE) or not set (FALSE).&#xA0; Now that we have 64 bit compilers using an int variable for booleans, there is *one* value which is FALSE (zero) and 2**64-1 values that are TRUE (everything else).&#xA0; It appears there&apos;s a lot more truth in this universe, but false can trump anything that&apos;s true...<br><br>PHP&apos;s handling of strings as booleans is *almost* correct - an empty string is FALSE, and a non-empty string is TRUE - with one exception:&#xA0; A string containing a single zero is considered FALSE.&#xA0; Why?&#xA0; If *any* non-empty strings are going to be considered FALSE, why *only* a single zero?&#xA0; Why not &quot;FALSE&quot; (preferably case insensitive), or &quot;0.0&quot; (with how many decimal places), or &quot;NO&quot; (again, case insensitive), or ... ?<br><br>The *correct* design would have been that *any* non-empty string is TRUE - period, end of story.&#xA0; Instead, there&apos;s another GOTCHA for the less-than-completely-experienced programmer to watch out for, and fixing the language&apos;s design error at this late date would undoubtedly break so many things that the correction is completely out of the question.<br><br>Speaking of GOTCHAs, consider this code sequence:<br><span class="default">&lt;?php<br>$x</span><span class="keyword">=</span><span class="default">TRUE</span><span class="keyword">;<br></span><span class="default">$y</span><span class="keyword">=</span><span class="default">FALSE</span><span class="keyword">;<br></span><span class="default">$z</span><span class="keyword">=</span><span class="default">$y </span><span class="keyword">OR </span><span class="default">$x</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Is $z TRUE or FALSE?<br><br>In this case, $z will be FALSE because the above code is equivalent to <span class="default">&lt;?php </span><span class="keyword">(</span><span class="default">$z</span><span class="keyword">=</span><span class="default">$y</span><span class="keyword">) OR </span><span class="default">$x ?&gt;</span> rather than <span class="default">&lt;?php $z</span><span class="keyword">=(</span><span class="default">$y </span><span class="keyword">OR </span><span class="default">$x</span><span class="keyword">) </span><span class="default">?&gt;</span> as might be expected - because the OR operator has lower precedence than assignment operators.<br><br>On the other hand, after this code sequence:<br><span class="default">&lt;?php<br>$x</span><span class="keyword">=</span><span class="default">TRUE</span><span class="keyword">;<br></span><span class="default">$y</span><span class="keyword">=</span><span class="default">FALSE</span><span class="keyword">;<br></span><span class="default">$z</span><span class="keyword">=</span><span class="default">$y </span><span class="keyword">|| </span><span class="default">$x</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>$z will be TRUE, as expected, because the || operator has higher precedence than assignment:&#xA0; The code is equivalent to $z=($y OR $x).<br><br>This is why you should NEVER use the OR operator without explicit parentheses around the expression where it is being used.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note for JavaScript developers:<br><br>In PHP, an empty array evaluates to false, while in JavaScript an empty array evaluates to true.<br><br>In PHP, you can test an empty array as <span class="default">&lt;?php </span><span class="keyword">if(!</span><span class="default">$stuff</span><span class="keyword">) </span><span class="default">&#x2026;</span><span class="keyword">; </span><span class="default">?&gt;</span> which won&#x2019;t work in JavaScript where you need to test the array length.<br><br>This is because in JavaScript, an array is an object, and, while it may not have any elements, it is still regarded as something.<br><br>Just a trap for young players who routinely work in both langauges.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware of certain control behavior with boolean and non boolean values :<br><br><span class="default">&lt;?php<br></span><span class="comment">// Consider that the 0 could by any parameters including itself<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">0 </span><span class="keyword">== </span><span class="default">1</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">0 </span><span class="keyword">== (bool)</span><span class="string">&apos;all&apos;</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">0 </span><span class="keyword">== </span><span class="string">&apos;all&apos;</span><span class="keyword">); </span><span class="comment">// TRUE, take care<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">0 </span><span class="keyword">=== </span><span class="string">&apos;all&apos;</span><span class="keyword">); </span><span class="comment">// false<br><br>// To avoid this behavior, you need to cast your parameter as string like that :<br></span><span class="default">var_dump</span><span class="keyword">((string)</span><span class="default">0 </span><span class="keyword">== </span><span class="string">&apos;all&apos;</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just something that will probably save time for many new developers: beware of interpreting FALSE and TRUE as integers. <br>For example, a small function for deleting elements of an array may give unexpected results if you are not fully aware of what happens: <br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">remove_element</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">)<br>{<br>&#xA0;&#xA0; </span><span class="comment">//array_search returns index of element, and FALSE if nothing is found<br>&#xA0;&#xA0; </span><span class="default">$index </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);<br>&#xA0;&#xA0; unset (</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]);<br>&#xA0;&#xA0; return </span><span class="default">$array</span><span class="keyword">; <br>}<br><br></span><span class="comment">// this will remove element &apos;A&apos;<br></span><span class="default">$array </span><span class="keyword">= [</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;B&apos;</span><span class="keyword">, </span><span class="string">&apos;C&apos;</span><span class="keyword">];<br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">remove_element</span><span class="keyword">(</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);<br><br></span><span class="comment">//but any non-existent element will also remove &apos;A&apos;!<br></span><span class="default">$array </span><span class="keyword">= [</span><span class="string">&apos;A&apos;</span><span class="keyword">, </span><span class="string">&apos;B&apos;</span><span class="keyword">, </span><span class="string">&apos;C&apos;</span><span class="keyword">];<br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">remove_element</span><span class="keyword">(</span><span class="string">&apos;X&apos;</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The problem here is, although array_search returns boolean false when it doesn&apos;t find specific element, it is interpreted as zero when used as array index.<br><br>So you have to explicitly check for FALSE, otherwise you&apos;ll probably loose some elements:<br><br><span class="default">&lt;?php<br></span><span class="comment">//correct<br></span><span class="keyword">function </span><span class="default">remove_element</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">)<br>{<br>&#xA0;&#xA0; </span><span class="default">$index </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="default">$element</span><span class="keyword">, </span><span class="default">$array</span><span class="keyword">);<br>&#xA0;&#xA0; if (</span><span class="default">$index </span><span class="keyword">!== </span><span class="default">FALSE</span><span class="keyword">) <br>&#xA0;&#xA0; {<br>&#xA0; &#xA0; &#xA0;&#xA0; unset (</span><span class="default">$array</span><span class="keyword">[</span><span class="default">$index</span><span class="keyword">]);<br>&#xA0;&#xA0; }<br>&#xA0;&#xA0; return </span><span class="default">$array</span><span class="keyword">; <br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware that &quot;0.00&quot; converts to boolean TRUE !<br><br>You may get such a string from your database, if you have columns of type DECIMAL or CURRENCY. In such cases you have to explicitly check if the value is != 0 or to explicitly convert the value to int also, not only to boolean.</span>
</div>
  

#


<div class="phpcode"><span class="html">
PHP does not break any rules with the values of true and false.&#xA0; The value false is not a constant for the number 0, it is a boolean value that indicates false.&#xA0; The value true is also not a constant for 1, it is a special boolean value that indicates true.&#xA0; It just happens to cast to integer 1 when you print it or use it in an expression, but it&apos;s not the same as a constant for the integer value 1 and you shouldn&apos;t use it as one.&#xA0; Notice what it says at the top of the page:<br><br>A boolean expresses a truth value.<br><br>It does not say &quot;a boolean expresses a 0 or 1&quot;.<br><br>It&apos;s true that symbolic constants are specifically designed to always and only reference their constant value.&#xA0; But booleans are not symbolic constants, they are values.&#xA0; If you&apos;re trying to add 2 boolean values you might have other problems in your application.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is correct that TRUE or FALSE should not be used as constants for the numbers 0 and 1. But there may be times when it might be helpful to see the value of the Boolean as a 1 or 0. Here&apos;s how to do it.
<br>
<br><span class="default">&lt;?php
<br>$var1 </span><span class="keyword">= </span><span class="default">TRUE</span><span class="keyword">;
<br></span><span class="default">$var2 </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">;
<br>
<br>echo </span><span class="default">$var1</span><span class="keyword">; </span><span class="comment">// Will display the number 1
<br>
<br></span><span class="keyword">echo </span><span class="default">$var2</span><span class="keyword">; </span><span class="comment">//Will display nothing
<br>
<br>/* To get it to display the number 0 for
<br>a false value you have to typecast it: */
<br>
<br></span><span class="keyword">echo (int)</span><span class="default">$var2</span><span class="keyword">; </span><span class="comment">//This will display the number 0 for false.
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note you can also use the &apos;!&apos; to convert a number to a boolean, as if it was an explicit (bool) cast then NOT.<br><br>So you can do something like:<br><br><span class="default">&lt;?php<br>$t </span><span class="keyword">= !</span><span class="default">0</span><span class="keyword">; </span><span class="comment">// This will === true;<br></span><span class="default">$f </span><span class="keyword">= !</span><span class="default">1</span><span class="keyword">; </span><span class="comment">// This will === false;<br></span><span class="default">?&gt;<br></span><br>And non-integers are casted as if to bool, then NOT.<br><br>Example:<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= !array();&#xA0; &#xA0; &#xA0; </span><span class="comment">// This will === true;<br></span><span class="default">$a </span><span class="keyword">= !array(</span><span class="string">&apos;a&apos;</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">// This will === false;<br></span><span class="default">$s </span><span class="keyword">= !</span><span class="string">&quot;&quot;</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// This will === true;<br></span><span class="default">$s </span><span class="keyword">= !</span><span class="string">&quot;hello&quot;</span><span class="keyword">;&#xA0; &#xA0; &#xA0; </span><span class="comment">// This will === false;<br></span><span class="default">?&gt;<br></span><br>To cast as if using a (bool) you can NOT the NOT with &quot;!!&quot; (double &apos;!&apos;), then you are casting to the correct (bool).<br><br>Example:<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= !!array();&#xA0;&#xA0; </span><span class="comment">// This will === false; (as expected)<br>/* <br>This can be a substitute for count($array) &gt; 0 or !(empty($array)) to check to see if an array is empty or not&#xA0; (you would use: !!$array).<br>*/<br><br></span><span class="default">$status </span><span class="keyword">= (!!</span><span class="default">$array </span><span class="keyword">? </span><span class="string">&apos;complete&apos; </span><span class="keyword">: </span><span class="string">&apos;incomplete&apos;</span><span class="keyword">);<br><br></span><span class="default">$s </span><span class="keyword">= !!</span><span class="string">&quot;testing&quot;</span><span class="keyword">; </span><span class="comment">// This will === true; (as expected)<br>/* <br>Note: normal casting rules apply so a !!&quot;0&quot; would evaluate to an === false<br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.boolean.php)

**[⬆ to root](/)**