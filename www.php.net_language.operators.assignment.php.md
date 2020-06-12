# Assignment Operators




<div class="phpcode"><span class="html">
This page really ought to have table of assignment operators,<br>namely,<br><br>See the Arithmetic Operators page (<a href="http://www.php.net/manual/en/language.operators.arithmetic.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/language.operators.arithmetic.php</a>)<br>Assignment&#xA0; &#xA0; Same as:<br>$a += $b&#xA0; &#xA0;&#xA0; $a = $a + $b&#xA0; &#xA0; Addition<br>$a -= $b&#xA0; &#xA0;&#xA0; $a = $a - $b&#xA0; &#xA0;&#xA0; Subtraction<br>$a *= $b&#xA0; &#xA0;&#xA0; $a = $a * $b&#xA0; &#xA0;&#xA0; Multiplication<br>$a /= $b&#xA0; &#xA0;&#xA0; $a = $a / $b&#xA0; &#xA0; Division<br>$a %= $b&#xA0; &#xA0;&#xA0; $a = $a % $b&#xA0; &#xA0; Modulus<br><br>See the String Operators page(<a href="http://www.php.net/manual/en/language.operators.string.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/language.operators.string.php</a>)<br>$a .= $b&#xA0; &#xA0;&#xA0; $a = $a . $b&#xA0; &#xA0; &#xA0;&#xA0; Concatenate<br><br>See the Bitwise Operators page (<a href="http://www.php.net/manual/en/language.operators.bitwise.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/language.operators.bitwise.php</a>)<br>$a &amp;= $b&#xA0; &#xA0;&#xA0; $a = $a &amp; $b&#xA0; &#xA0;&#xA0; Bitwise And<br>$a |= $b&#xA0; &#xA0;&#xA0; $a = $a | $b&#xA0; &#xA0; &#xA0; Bitwise Or<br>$a ^= $b&#xA0; &#xA0;&#xA0; $a = $a ^ $b&#xA0; &#xA0; &#xA0;&#xA0; Bitwise Xor<br>$a &lt;&lt;= $b&#xA0; &#xA0;&#xA0; $a = $a &lt;&lt; $b&#xA0; &#xA0;&#xA0; Left shift<br>$a &gt;&gt;= $b&#xA0; &#xA0;&#xA0; $a = $a &gt;&gt; $b&#xA0; &#xA0; &#xA0; Right shift</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using $text .= &quot;additional text&quot;; instead of $text =&#xA0; $text .&quot;additional text&quot;; can seriously enhance performance due to memory allocation efficiency. <br><br>I reduced execution time from 5 sec to .5 sec (10 times) by simply switching to the first pattern for a loop with 900 iterations over a string $text that reaches 800K by the end.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be aware of assignments with conditionals. The assignment operator is stronger as &apos;and&apos;, &apos;or&apos; and &apos;xor&apos;.<br><br><span class="default">&lt;?php <br>$x </span><span class="keyword">= </span><span class="default">true </span><span class="keyword">and </span><span class="default">false</span><span class="keyword">;&#xA0;&#xA0; </span><span class="comment">//$x will be true<br></span><span class="default">$y </span><span class="keyword">= (</span><span class="default">true </span><span class="keyword">and </span><span class="default">false</span><span class="keyword">); </span><span class="comment">//$y will be false<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
bradlis7 at bradlis7 dot com&apos;s description is a bit confusing. Here it is rephrased.<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="string">&apos;a&apos;</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">= </span><span class="string">&apos;b&apos;</span><span class="keyword">;<br><br></span><span class="default">$a </span><span class="keyword">.= </span><span class="default">$b </span><span class="keyword">.= </span><span class="string">&quot;foo&quot;</span><span class="keyword">;<br><br>echo </span><span class="default">$a</span><span class="keyword">,</span><span class="string">&quot;\n&quot;</span><span class="keyword">,</span><span class="default">$b</span><span class="keyword">;</span><span class="default">?&gt;<br></span>outputs<br><br>abfoo<br>bfoo<br><br>Because the assignment operators are right-associative and evaluate to the result of the assignment<br><span class="default">&lt;?php<br>$a </span><span class="keyword">.= </span><span class="default">$b </span><span class="keyword">.= </span><span class="string">&quot;foo&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>is equivalent to<br><span class="default">&lt;?php<br>$a </span><span class="keyword">.= (</span><span class="default">$b </span><span class="keyword">.= </span><span class="string">&quot;foo&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>and therefore<br><span class="default">&lt;?php<br>$b </span><span class="keyword">.= </span><span class="string">&quot;foo&quot;</span><span class="keyword">;<br></span><span class="default">$a </span><span class="keyword">.= </span><span class="default">$b</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.assignment.php)

**[To root](/)**