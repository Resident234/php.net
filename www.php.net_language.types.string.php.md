# Strings




<div class="phpcode"><span class="html">
I&apos;ve been a PHP programmer for a decade, and I&apos;ve always been using the &quot;single-quoted literal&quot; and &quot;period-concatenation&quot; method of string creation. But I wanted to answer the performance question once and for all, using sufficient numbers of iterations and a modern PHP version. For my test, I used:
<br>
<br>php -v
<br>PHP 7.0.12 (cli) (built: Oct 14 2016 09:56:59) ( NTS )
<br>Copyright (c) 1997-2016 The PHP Group
<br>Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies
<br>
<br>------ Results: -------
<br>
<br>* 100 million iterations:
<br>
<br>$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n;
<br>63608ms (34.7% slower)
<br>
<br>$outstr = &quot;literal$n$data$int$data$float$n&quot;;
<br>47218ms (fastest)
<br>
<br>$outstr =&lt;&lt;&lt;EOS
<br>literal$n$data$int$data$float$n
<br>EOS;
<br>47992ms (1.64% slower)
<br>
<br>$outstr = sprintf(&apos;literal%s%s%d%s%f%s&apos;, $n, $data, $int, $data, $float, $n);
<br>76629ms (62.3% slower)
<br>
<br>$outstr = sprintf(&apos;literal%s%5$s%2$d%3$s%4$f%s&apos;, $n, $int, $data, $float, $data, $n);
<br>96260ms (103.9% slower)
<br>
<br>* 10 million iterations (test adapted to see which of the two fastest methods were faster at adding a newline; either the PHP_EOL literal, or the \n string expansion):
<br>
<br>$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n;
<br>6228ms (reference for single-quoted without newline)
<br>
<br>$outstr = &quot;literal$n$data$int$data$float$n&quot;;
<br>4653ms (reference for double-quoted without newline)
<br>
<br>$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n . PHP_EOL;
<br>6630ms (35.3% slower than double-quoted with \n newline)
<br>
<br>$outstr = &quot;literal$n$data$int$data$float$n\n&quot;;
<br>4899ms (fastest at newlines)
<br>
<br>* 100 million iterations (a test intended to see which one of the two ${var} and {$var} double-quote styles is faster):
<br>
<br>$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n;
<br>67048ms (38.2% slower)
<br>
<br>$outstr = &quot;literal$n$data$int$data$float$n&quot;;
<br>49058ms (1.15% slower)
<br>
<br>$outstr = &quot;literal{$n}{$data}{$int}{$data}{$float}{$n}&quot;
<br>49221ms (1.49% slower)
<br>
<br>$outstr = &quot;literal${n}${data}${int}${data}${float}${n}&quot;
<br>48500ms (fastest; the differences are small but this held true across multiple runs of the test, and this was always the fastest variable encapsulation style)
<br>
<br>* 1 BILLION iterations (testing a completely literal string with nothing to parse in it):
<br>
<br>$outstr = &apos;literal string testing&apos;;
<br>23852ms (fastest)
<br>
<br>$outstr = &quot;literal string testing&quot;;
<br>24222ms (1.55% slower)
<br>
<br>It blows my mind. The double-quoted strings &quot;which look so $slow since they have to parse everything for \n backslashes and $dollar signs to do variable expansion&quot;, turned out to be the FASTEST string concatenation method in PHP - PERIOD!
<br>
<br>Single-quotes are only faster if your string is completely literal (with nothing to parse in it and nothing to concatenate), but the margin is very tiny and doesn&apos;t matter.
<br>
<br>So the &quot;highest code performance&quot; style rules are:
<br>
<br>1. Always use double-quoted strings for concatenation.
<br>
<br>2. Put your variables in &quot;This is a {$variable} notation&quot;, because it&apos;s the fastest method which still allows complex expansions like &quot;This {$var[&apos;foo&apos;]} is {$obj-&gt;awesome()}!&quot;. You cannot do that with the &quot;${var}&quot; style.
<br>
<br>3. Feel free to use single-quoted strings for TOTALLY literal strings such as array keys/values, variable values, etc, since they are a TINY bit faster when you want literal non-parsed strings. But I had to do 1 billion iterations to find a 1.55% measurable difference. So the only real reason I&apos;d consider using single-quoted strings for my literals is for code cleanliness, to make it super clear that the string is literal.
<br>
<br>4. If you think another method such as sprintf() or &apos;this&apos;.$var.&apos;style&apos; is more readable, and you don&apos;t care about maximizing performance, then feel free to use whatever concatenation method you prefer!</span>
</div>
  

#


<div class="phpcode"><span class="html">
The documentation does not mention, but a closing semicolon at the end of the heredoc is actually interpreted as a real semicolon, and as such, sometimes leads to syntax errors.<br><br>This works:<br><br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= &lt;&lt;&lt;END<br></span><span class="string">abcd<br></span><span class="keyword">END;<br></span><span class="default">?&gt;<br></span><br>This does not:<br><br><span class="default">&lt;?php<br>foo</span><span class="keyword">(&lt;&lt;&lt;END<br></span><span class="string">abcd<br></span><span class="keyword">END;<br>);<br></span><span class="comment">// syntax error, unexpected &apos;;&apos;<br></span><span class="default">?&gt;<br></span><br>Without semicolon, it works fine:<br><br><span class="default">&lt;?php<br>foo</span><span class="keyword">(&lt;&lt;&lt;END<br></span><span class="string">abcd<br></span><span class="keyword">END<br>);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
md5(&apos;240610708&apos;) == md5(&apos;QNKCDZO&apos;)<br><br>This comparison is true because both md5() hashes start &apos;0e&apos; so PHP type juggling understands these strings to be scientific notation.&#xA0; By definition, zero raised to any power is zero.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use string like array of char (like C)<br><br>$a = &quot;String array test&quot;;<br><br>var_dump($a); <br>// Return string(17) &quot;String array test&quot;<br><br>var_dump($a[0]); <br>// Return string(1) &quot;S&quot;<br><br>// -- With array cast --<br>var_dump((array) $a); <br>// Return array(1) { [0]=&gt; string(17) &quot;String array test&quot;}<br><br>var_dump((array) $a[0]); <br>// Return string(17) &quot;S&quot;<br><br>- Norihiori</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use the complex syntax to put the value of both object properties AND object methods inside a string.&#xA0; For example...<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Test </span><span class="keyword">{<br>&#xA0; &#xA0; public </span><span class="default">$one </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>&#xA0; &#xA0; public function </span><span class="default">two</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">2</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">$test </span><span class="keyword">= new </span><span class="default">Test</span><span class="keyword">();<br>echo </span><span class="string">&quot;foo </span><span class="keyword">{</span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">one</span><span class="keyword">}</span><span class="string"> bar </span><span class="keyword">{</span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">two</span><span class="keyword">()}</span><span class="string">&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>Will output &quot;foo 1 bar 2&quot;.<br><br>However, you cannot do this for all values in your namespace.&#xA0; Class constants and static properties/methods will not work because the complex syntax looks for the &apos;$&apos;.<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Test </span><span class="keyword">{<br>&#xA0; &#xA0; const </span><span class="default">ONE </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br>}<br>echo </span><span class="string">&quot;foo {Test::ONE} bar&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>This will output &quot;foo {Test::one} bar&quot;.&#xA0; Constants and static properties require you to break up the string.</span>
</div>
  

#


<div class="phpcode"><span class="html">
easy transparent solution for using constants in the heredoc format:<br>DEFINE(&apos;TEST&apos;,&apos;TEST STRING&apos;);<br><br>$const = get_defined_constants();<br><br>echo &lt;&lt;&lt;END<br>{$const[&apos;TEST&apos;]}<br>END;<br><br>Result:<br>TEST STRING</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is an easy hack to allow double-quoted strings and heredocs to contain arbitrary expressions in curly braces syntax, including constants and other function calls:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// Hack declaration<br></span><span class="keyword">function </span><span class="default">_expr</span><span class="keyword">(</span><span class="default">$v</span><span class="keyword">) { return </span><span class="default">$v</span><span class="keyword">; }<br></span><span class="default">$_expr </span><span class="keyword">= </span><span class="string">&apos;_expr&apos;</span><span class="keyword">;<br><br></span><span class="comment">// Our playground<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;qwe&apos;</span><span class="keyword">, </span><span class="string">&apos;asd&apos;</span><span class="keyword">);<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;zxc&apos;</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">);<br><br></span><span class="default">$a</span><span class="keyword">=</span><span class="default">3</span><span class="keyword">;<br></span><span class="default">$b</span><span class="keyword">=</span><span class="default">4</span><span class="keyword">;<br><br>function </span><span class="default">c</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">) { return </span><span class="default">$a</span><span class="keyword">+</span><span class="default">$b</span><span class="keyword">; }<br><br></span><span class="comment">// Usage<br></span><span class="keyword">echo </span><span class="string">&quot;pre </span><span class="keyword">{</span><span class="default">$_expr</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">+</span><span class="default">2</span><span class="keyword">)}</span><span class="string"> post\n&quot;</span><span class="keyword">; </span><span class="comment">// outputs &apos;pre 3 post&apos;<br></span><span class="keyword">echo </span><span class="string">&quot;pre </span><span class="keyword">{</span><span class="default">$_expr</span><span class="keyword">(</span><span class="default">qwe</span><span class="keyword">)}</span><span class="string"> post\n&quot;</span><span class="keyword">; </span><span class="comment">// outputs &apos;pre asd post&apos;<br></span><span class="keyword">echo </span><span class="string">&quot;pre </span><span class="keyword">{</span><span class="default">$_expr</span><span class="keyword">(</span><span class="default">c</span><span class="keyword">(</span><span class="default">$a</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">)+</span><span class="default">zxc</span><span class="keyword">*</span><span class="default">2</span><span class="keyword">)}</span><span class="string"> post\n&quot;</span><span class="keyword">; </span><span class="comment">// outputs &apos;pre 17 post&apos;<br><br>// General syntax is {$_expr(...)}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To save Your mind don&apos;t read previous comments about dates&#xA0; ;)<br><br>When both strings can be converted to the numerics (in (&quot;$a&quot; &gt; &quot;$b&quot;) test) then resulted numerics are used, else FULL strings are compared char-by-char:<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="string">&apos;1.22&apos; </span><span class="keyword">&gt; </span><span class="string">&apos;01.23&apos;</span><span class="keyword">); </span><span class="comment">// bool(false)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;1.22.00&apos; </span><span class="keyword">&gt; </span><span class="string">&apos;01.23.00&apos;</span><span class="keyword">); </span><span class="comment">// bool(true)<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="string">&apos;1-22-00&apos; </span><span class="keyword">&gt; </span><span class="string">&apos;01-23-00&apos;</span><span class="keyword">); </span><span class="comment">// bool(true)<br></span><span class="default">var_dump</span><span class="keyword">((float)</span><span class="string">&apos;1.22.00&apos; </span><span class="keyword">&gt; (float)</span><span class="string">&apos;01.23.00&apos;</span><span class="keyword">); </span><span class="comment">// bool(false)<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.string.php)

**[To root](/README.md)**