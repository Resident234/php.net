# boolval




<div class="phpcode"><span class="html">
&lt;?<br><br>// Hack for old php versions to use boolval()<br><br>if (!function_exists(&apos;boolval&apos;)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; function boolval($val) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return (bool) $val;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>}<br>?&gt;</span>
</div>
  

#


<div class="phpcode"><span class="html">
To anyone like me who came here looking for a way to turn any value into a 0/1 that will fit into a MySQL boolean (tinyint) field:<br><br><span class="default">&lt;?php<br>$tinyint </span><span class="keyword">= (int) </span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$valToCheck</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_BOOLEAN</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>tinyint will be 0 (zero) for values like string &quot;false&quot;, boolean false, int 0<br><br>tinyint will be 1 for values like string &quot;true&quot;, boolean true, int 1<br><br>Useful if you are accepting data that might be from a language like Javascript that sends string &quot;false&quot; for a boolean false.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I believe that the double negation !! can perform the same task with the same result if your PHP is not up2date<br><br>var_dump(!!1, !!0, !!&quot;test&quot;, !!&quot;&quot;);<br><br>outputs:<br>boolean true<br><br>boolean false<br><br>boolean true<br><br>boolean false<br><br>May the life be good to you.</span>
</div>
  

#


<div class="phpcode"><span class="html">
function is_true($val, $return_null=false){<br>&#xA0; &#xA0; $boolval = ( is_string($val) ? filter_var($val, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE) : (bool) $val );<br>&#xA0; &#xA0; return ( $boolval===null &amp;&amp; !$return_null ? false : $boolval );<br>}<br><br>// Return Values:<br><br>is_true(new stdClass);&#xA0; &#xA0; &#xA0; // true<br>is_true([1,2]);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true<br>is_true([1]);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true<br>is_true([0]);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true<br>is_true(42);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // true<br>is_true(-42);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true<br>is_true(&apos;true&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // true<br>is_true(&apos;on&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true<br>is_true(&apos;off&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false<br>is_true(&apos;yes&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // true<br>is_true(&apos;no&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;ja&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;nein&apos;)&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;1&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // true<br>is_true(NULL);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false<br>is_true(0);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;false&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;string&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false<br>is_true(&apos;0.0&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;4.2&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;0&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // false<br>is_true(&apos;&apos;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false<br>is_true([]);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // false</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.boolval.php)

**[To root](/README.md)**