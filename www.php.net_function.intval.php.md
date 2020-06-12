# intval




<div class="phpcode"><span class="html">
Not mentioned elsewhere: intval(NULL) also returns 0.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It seems intval is interpreting valid numeric strings differently between PHP 5.6 and 7.0 on one hand, and PHP 7.1 on the other hand.<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">intval</span><span class="keyword">(</span><span class="string">&apos;1e5&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>will return 1 on PHP 5.6 and PHP 7.0,<br>but it will return 100000 on PHP 7.1.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful :<br><br><span class="default">&lt;?php<br>$n</span><span class="keyword">=</span><span class="string">&quot;19.99&quot;</span><span class="keyword">;<br>print </span><span class="default">intval</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">*</span><span class="default">100</span><span class="keyword">); </span><span class="comment">// prints 1998<br></span><span class="keyword">print </span><span class="default">intval</span><span class="keyword">(</span><span class="default">strval</span><span class="keyword">(</span><span class="default">$n</span><span class="keyword">*</span><span class="default">100</span><span class="keyword">)); </span><span class="comment">// prints 1999<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
intval converts doubles to integers by truncating the fractional component of the number.
<br>
<br>When dealing with some values, this can give odd results.&#xA0; Consider the following:
<br>
<br>print intval ((0.1 + 0.7) * 10);
<br>
<br>This will most likely print out 7, instead of the expected value of 8.
<br>
<br>For more information, see the section on floating point numbers in the PHP manual (<a href="http://www.php.net/manual/language.types.double.php" rel="nofollow" target="_blank">http://www.php.net/manual/language.types.double.php</a>)
<br>
<br>Also note that if you try to convert a string to an integer, the result is often 0.
<br>
<br>However, if the leftmost character of a string looks like a valid numeric value, then PHP will keep reading the string until a character that is not valid in a number is encountered.
<br>
<br>For example:
<br>
<br>&quot;101 Dalmations&quot; will convert to 101
<br>
<br>&quot;$1,000,000&quot; will convert to 0 (the 1st character is not a valid start for a number
<br>
<br>&quot;80,000 leagues ...&quot; will convert to 80
<br>
<br>&quot;1.4e98 microLenats were generated when...&quot; will convert to 1.4e98
<br>
<br>Also note that only decimal base numbers are recognized in strings.
<br>
<br>&quot;099&quot; will convert to 99, while &quot;0x99&quot; will convert to 0.
<br>
<br>One additional note on the behavior of intval.&#xA0; If you specify the base argument, the var argument should be a string - otherwise the base will not be applied.
<br>
<br>For Example:
<br>
<br>print intval (77, 8);&#xA0;&#xA0; // Prints 77
<br>print intval (&apos;77&apos;, 8); // Prints 63</span>
</div>
  

#


<div class="phpcode"><span class="html">
You guys are going to love this.&#xA0; I found something that I found quite disturbing.<br><br>$test1 = intVal(1999);<br><br>$amount = 19.99 * 100;<br>$test2 = intVal($amount);<br>$test3 = intVal(&quot;$amount&quot;);<br><br>echo $test1 . &quot;&lt;br /&gt;\n&quot;;<br>echo $test2 . &quot;&lt;br /&gt;\n&quot;;<br>echo $test3 . &quot;&lt;br /&gt;\n&quot;;<br><br>expected output:<br>1999<br>1999<br>1999<br><br>actual output<br>1999<br>1998<br>1999<br><br>Appears to be a floating point issue, but the number 1999 is the only number that I was able to get to do this.&#xA0; 19.99 is the price of many things, and for our purpose we must pass it as 1999 instead of 19.99.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a really useful undocumented feature:
<br>
<br>You can have it automatically deduce the base of the number from the prefix of the string using the same syntax as integer literals in PHP (&quot;0x&quot; for hexadecimal, &quot;0&quot; for octal, non-&quot;0&quot; for decimal) by passing a base of 0 to intval():
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">echo </span><span class="default">intval</span><span class="keyword">(</span><span class="string">&quot;0x1a&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">; </span><span class="comment">// hex; prints &quot;26&quot;
<br></span><span class="keyword">echo </span><span class="default">intval</span><span class="keyword">(</span><span class="string">&quot;057&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">; </span><span class="comment">// octal; prints &quot;47&quot;
<br></span><span class="keyword">echo </span><span class="default">intval</span><span class="keyword">(</span><span class="string">&quot;42&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">), </span><span class="string">&quot;\n&quot;</span><span class="keyword">; </span><span class="comment">// decimal; prints &quot;42&quot;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you want to take a number from a string, no matter what it may contain, here is a good solution:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">int</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">){return(int)</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/[^\-\d]*(\-?\d*).*/&apos;</span><span class="keyword">,</span><span class="string">&apos;$1&apos;</span><span class="keyword">,</span><span class="default">$s</span><span class="keyword">);}<br><br>echo </span><span class="default">int</span><span class="keyword">(</span><span class="string">&apos;j18ugj9hu0gj5hg&apos;</span><span class="keyword">);<br></span><span class="comment">//output: 18<br></span><span class="default">?&gt;<br></span>this example returns an int, so it will follow the int rules, and has support for negative values.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">int</span><span class="keyword">(</span><span class="default">$s</span><span class="keyword">){return(</span><span class="default">$a</span><span class="keyword">=</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/[^\-\d]*(\-?\d*).*/&apos;</span><span class="keyword">,</span><span class="string">&apos;$1&apos;</span><span class="keyword">,</span><span class="default">$s</span><span class="keyword">))?</span><span class="default">$a</span><span class="keyword">:</span><span class="string">&apos;0&apos;</span><span class="keyword">;}<br><br>echo </span><span class="default">int</span><span class="keyword">(</span><span class="string">&apos;j-1809809808908099878758765ugj9hu0gj5hg&apos;</span><span class="keyword">);<br></span><span class="comment">//output: -1809809808908099878758765<br></span><span class="default">?&gt;<br></span><br>this one returns a string with just the numeric value.<br>it also supports negative values.<br><br>the latter is better when you have a 32 bit system and you want a huge int that is higher than PHP_MAX_INT.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.intval.php)

**[⬆ to root](/)**