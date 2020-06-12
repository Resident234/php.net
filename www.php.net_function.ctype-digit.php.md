# ctype_digit




<div class="phpcode"><span class="html">
All basic PHP functions which i tried returned unexpected results. I would just like to check whether some variable only contains numbers. For example: when i spread my script to the public i cannot require users to only use numbers as string or as integer. For those situation i wrote my own function which handles all inconveniences of other functions and which is not depending on regular expressions. Some people strongly believe that regular functions slow down your script.<br><br>The reason to write this function:<br>1. is_numeric() accepts values like: +0123.45e6 (but you would expect it would not)<br>2. is_int() does not accept HTML form fields (like: 123) because they are treated as strings (like: &quot;123&quot;).<br>3. ctype_digit() excepts all numbers to be strings (like: &quot;123&quot;) and does not validate real integers (like: 123).<br>4. Probably some functions would parse a boolean (like: true or false) as 0 or 1 and validate it in that manner.<br><br>My function only accepts numbers regardless whether they are in string or in integer format.<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0;&#xA0; * Check input for existing only of digits (numbers)<br>&#xA0; &#xA0;&#xA0; * @author Tim Boormans &lt;info@directwebsolutions.nl&gt;<br>&#xA0; &#xA0;&#xA0; * @param $digit<br>&#xA0; &#xA0;&#xA0; * @return bool<br>&#xA0; &#xA0;&#xA0; */<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">is_digit</span><span class="keyword">(</span><span class="default">$digit</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">is_int</span><span class="keyword">(</span><span class="default">$digit</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; } elseif(</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$digit</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">ctype_digit</span><span class="keyword">(</span><span class="default">$digit</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// booleans, floats and others<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I just wanted to clarify a flaw in the function is_digit() suggested by &quot;info at directwebsolutions dot nl &quot; .. <br>It returns true in case of negative integers and false in case of strings that contain negative integers .<br> example:<br>is_digit(-10); // returns ture<br>is_digit(&apos;-10&apos;); // returns false</span>
</div>
  

#


<div class="phpcode"><span class="html">
Also note that
<br>
<br><span class="default">&lt;?php ctype_digit</span><span class="keyword">(</span><span class="string">&quot;-1&quot;</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">//false </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Interesting to note that you must pass a STRING to this function, other values won&apos;t be typecasted (I figured it would even though above explicitly says string $text).<br><br>I.E.<br><br><span class="default">&lt;?PHP<br>$val </span><span class="keyword">= </span><span class="default">42</span><span class="keyword">; </span><span class="comment">//Answer to life<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">ctype_digit</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Will return false, even though, when typecasted to string, it would be true.<br><br><span class="default">&lt;?PHP<br>$val </span><span class="keyword">= </span><span class="string">&apos;42&apos;</span><span class="keyword">;<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">ctype_digit</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Returns True.<br><br>Could do this too:<br><br><span class="default">&lt;?PHP<br>$val </span><span class="keyword">= </span><span class="default">42</span><span class="keyword">;<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">ctype_digit</span><span class="keyword">((string) </span><span class="default">$val</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Which will also return true, as it should.</span>
</div>
  

#


<div class="phpcode"><span class="html">
ctype_digit() will treat all passed integers below 256 as character-codes. It returns true for 48 through 57 (ASCII &apos;0&apos;-&apos;9&apos;) and false for the rest.<br><br>ctype_digit(5) -&gt; false<br>ctype_digit(48) -&gt; true<br>ctype_digit(255) -&gt; false<br>ctype_digit(256) -&gt; true<br><br>(Note: the PHP type must be an int; if you pass strings it works as expected)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that an empty string is also false:<br>ctype_digit(&quot;&quot;) // false</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ctype-digit.php)

**[⬆ to root](/)**