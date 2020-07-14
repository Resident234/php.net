# PHP type comparison tables



[Editor&apos;s note: As of PHP 5.4.4 this is no longer true. Integral strings that overflow into floating point numbers will no longer be considered equal.]<br><br>Be wary of string-comparison where both strings might be interpreted as numbers.  Eg:<br><br>$x="123456789012345678901234567890"; $y="123456789012345678900000000000";<br>echo  ($x==$y)?"equal":"not_equal";   #Prints equal !!<br><br>Both strings are getting converted to floats, then losing precision, then becoming equal :-(<br><br>Using "===" or making either of the strings non-numeric will prevent this.<br>[This is on a 32-bit machine, on a 64-bit, you will have to make the strings longer to see the effect]  

#

It&apos;s interesting to note that &apos;empty()&apos; and &apos;boolean : if($x)&apos;<br>are paired as logical opposites, as are &apos;is_null()&apos; and &apos;isset()&apos;.  

#

A comparison table for &lt;=,&lt;,=&gt;,&gt; would be nice...<br>Following are TRUE (tested PHP4&amp;5):<br>NULL &lt;= -1<br>NULL &lt;= 0<br>NULL &lt;= 1<br>!(NULL &gt;= -1)<br>NULL &gt;= 0<br>!(NULL &gt;= 1)<br>That was a surprise for me (and it is not like SQL, I would like to have the option to have SQL semantics with NULL...).  

#

The way PHP handles comparisons when multiple types are concerned is quite confusing.<br><br>For example:<br>"php" == 0<br><br>This is true, because the string is casted interally to an integer. Any string (that does not start with a number), when casted to an integer, will be 0.  

#

Note that php comparison is not transitive:<br><br>"php" == 0 =&gt; true<br>0 == null =&gt; true<br>null == "php" =&gt; false  

#

[Official documentation page](https://www.php.net/manual/en/types.comparisons.php)

**[To root](/README.md)**