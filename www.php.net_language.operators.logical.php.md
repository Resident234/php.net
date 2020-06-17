# Logical Operators




<div class="phpcode"><span class="html">
Note that PHP&apos;s boolean operators *always* return a boolean value... as opposed to other languages that return the value of the last evaluated expression.<br><br>For example:<br><br>$a = 0 || &apos;avacado&apos;;<br>print &quot;A: $a\n&quot;;<br><br>will print:<br><br>A: 1<br><br>in PHP -- as opposed to printing &quot;A: avacado&quot; as it would in a language like Perl or JavaScript.<br><br>This means you can&apos;t use the &apos;||&apos; operator to set a default value:<br><br>$a = $fruit || &apos;apple&apos;;<br><br>instead, you have to use the &apos;?:&apos; operator:<br><br>$a = ($fruit ? $fruit : &apos;apple&apos;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
In addition to what Lawrence said about assigning a default value, one can now use the Null Coalescing Operator (PHP 7). Hence when we want to assign a default value we can write:<br><br>$a = ($fruit ?? &apos;apple&apos;); <br>//assigns the $fruit variable content to $a if the $fruit variable exists or has a value that is not NULL, or assigns the value &apos;apple&apos; to $a if the $fruit variable doesn&apos;t exists or it contains the NULL value</span>
</div>
  

#


<div class="phpcode"><span class="html">
This works similar to javascripts short-curcuit assignments and setting defaults. (e.g.&#xA0; var a = getParm() || &apos;a default&apos;;)<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">(</span><span class="default">$a </span><span class="keyword">= </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;var&apos;</span><span class="keyword">]) || (</span><span class="default">$a </span><span class="keyword">= </span><span class="string">&apos;a default&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>$a gets assigned $_GET[&apos;var&apos;] if there&apos;s anything in it or it will fallback to &apos;a default&apos;<br>Parentheses are required, otherwise you&apos;ll end up with $a being a boolean.</span>
</div>
  

#


<div class="phpcode"><span class="html">
worth reading for people learning about php and programming: (adding extras <span class="default">&lt;?php ?&gt;</span> to get highlighted code)<br><br>about the following example in this page manual: <br>Example#1 Logical operators illustrated<br><br>...<br><span class="default">&lt;?php<br></span><span class="comment">// &quot;||&quot; has a greater precedence than &quot;or&quot;<br></span><span class="default">$e </span><span class="keyword">= </span><span class="default">false </span><span class="keyword">|| </span><span class="default">true</span><span class="keyword">; </span><span class="comment">// $e will be assigned to (false || true) which is true<br></span><span class="default">$f </span><span class="keyword">= </span><span class="default">false </span><span class="keyword">or </span><span class="default">true</span><span class="keyword">; </span><span class="comment">// $f will be assigned to false<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$e</span><span class="keyword">, </span><span class="default">$f</span><span class="keyword">);<br><br></span><span class="comment">// &quot;&amp;&amp;&quot; has a greater precedence than &quot;and&quot;<br></span><span class="default">$g </span><span class="keyword">= </span><span class="default">true </span><span class="keyword">&amp;&amp; </span><span class="default">false</span><span class="keyword">; </span><span class="comment">// $g will be assigned to (true &amp;&amp; false) which is false<br></span><span class="default">$h </span><span class="keyword">= </span><span class="default">true </span><span class="keyword">and </span><span class="default">false</span><span class="keyword">; </span><span class="comment">// $h will be assigned to true<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$g</span><span class="keyword">, </span><span class="default">$h</span><span class="keyword">); <br></span><span class="default">?&gt;<br></span>_______________________________________________end of my quote...<br><br>If necessary, I wanted to give further explanation on this and say that when we write:<br>$f = false or true; // $f will be assigned to false<br>the explanation: <br><br>&quot;||&quot; has a greater precedence than &quot;or&quot; <br><br>its true. But a more acurate one would be<br><br>&quot;||&quot; has greater precedence than &quot;or&quot; and than &quot;=&quot;, whereas &quot;or&quot; doesnt have greater precedence than &quot;=&quot;, so<br><br><span class="default">&lt;?php<br>$f </span><span class="keyword">= </span><span class="default">false </span><span class="keyword">or </span><span class="default">true</span><span class="keyword">;<br><br></span><span class="comment">//is like writting<br><br></span><span class="keyword">(</span><span class="default">$f </span><span class="keyword">= </span><span class="default">false </span><span class="keyword">) or </span><span class="default">true</span><span class="keyword">;<br><br></span><span class="comment">//and<br><br></span><span class="default">$e </span><span class="keyword">= </span><span class="default">false </span><span class="keyword">|| </span><span class="default">true</span><span class="keyword">;<br><br></span><span class="default">is the same </span><span class="keyword">as<br><br></span><span class="default">$e </span><span class="keyword">= (</span><span class="default">false </span><span class="keyword">|| </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span> <br><br>same goes for &quot;&amp;&amp;&quot; and &quot;AND&quot;. <br><br>If you find it hard to remember operators precedence you can always use parenthesys - &quot;(&quot; and &quot;)&quot;. And even if you get to learn it remember that being a good programmer is not showing you can do code with fewer words. The point of being a good programmer is writting code that is easy to understand (comment your code when necessary!), easy to maintain and with high efficiency, among other things.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.logical.php)

**[To root](/README.md)**