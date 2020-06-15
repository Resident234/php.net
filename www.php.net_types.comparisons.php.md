# PHP type comparison tables




<div class="phpcode"><span class="html">
[Editor&apos;s note: As of PHP 5.4.4 this is no longer true. Integral strings that overflow into floating point numbers will no longer be considered equal.]
<br>
<br>Be wary of string-comparison where both strings might be interpreted as numbers.&#xA0; Eg:
<br>
<br>$x=&quot;123456789012345678901234567890&quot;; $y=&quot;123456789012345678900000000000&quot;;
<br>echo&#xA0; ($x==$y)?&quot;equal&quot;:&quot;not_equal&quot;;&#xA0;&#xA0; #Prints equal !!
<br>
<br>Both strings are getting converted to floats, then losing precision, then becoming equal :-(
<br>
<br>Using &quot;===&quot; or making either of the strings non-numeric will prevent this.
<br>[This is on a 32-bit machine, on a 64-bit, you will have to make the strings longer to see the effect]</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s interesting to note that &apos;empty()&apos; and &apos;boolean : if($x)&apos;<br>are paired as logical opposites, as are &apos;is_null()&apos; and &apos;isset()&apos;.</span>
</div>
  

#


<div class="phpcode"><span class="html">
A comparison table for &lt;=,&lt;,=&gt;,&gt; would be nice...<br>Following are TRUE (tested PHP4&amp;5):<br>NULL &lt;= -1<br>NULL &lt;= 0<br>NULL &lt;= 1<br>!(NULL &gt;= -1)<br>NULL &gt;= 0<br>!(NULL &gt;= 1)<br>That was a surprise for me (and it is not like SQL, I would like to have the option to have SQL semantics with NULL...).</span>
</div>
  

#


<div class="phpcode"><span class="html">
The way PHP handles comparisons when multiple types are concerned is quite confusing.<br><br>For example:<br>&quot;php&quot; == 0<br><br>This is true, because the string is casted interally to an integer. Any string (that does not start with a number), when casted to an integer, will be 0.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that php comparison is not transitive:<br><br>&quot;php&quot; == 0 =&gt; true<br>0 == null =&gt; true<br>null == &quot;php&quot; =&gt; false</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/types.comparisons.php)

**[To root](/README.md)**