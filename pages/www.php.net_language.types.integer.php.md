# Integers





A leading zero in a numeric literal means &quot;this is octal&quot;. But don&apos;t be confused: a leading zero in a string does not. Thus:
$x = 0123;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // 83
$y = &quot;0123&quot; + 0&#xA0; &#xA0;&#xA0; // 123

  

#



Here are some tricks to convert from a &quot;dotted&quot; IP address to a LONG int, and backwards. This is very useful because accessing an IP addy in a database table is very much faster if it&apos;s stored as a BIGINT rather than in characters.

IP to BIGINT:


```
<?php
&#xA0; $ipArr&#xA0; &#xA0; = explode(&apos;.&apos;,$_SERVER[&apos;REMOTE_ADDR&apos;]);
&#xA0; $ip&#xA0; &#xA0; &#xA0;&#xA0; = $ipArr[0] * 0x1000000
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; + $ipArr[1] * 0x10000
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; + $ipArr[2] * 0x100
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; + $ipArr[3]
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ;
?>
```


IP as BIGINT read from db back to dotted form:

Keep in mind, PHP integer operators are INTEGER -- not long. Also, since there is no integer divide in PHP, we save a couple of S-L-O-W floor (&lt;division&gt;)&apos;s by doing bitshifts. We must use floor(/) for $ipArr[0] because though $ipVal is stored as a long value, $ipVal &gt;&gt; 24 will operate on a truncated, integer value of $ipVal! $ipVint is, however, a nice integer, so 
we can enjoy the bitshifts.



```
<?php
&#xA0; &#xA0; &#xA0; &#xA0; $ipVal = $row[&apos;client_IP&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; $ipArr = array(0 =&gt;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; floor(&#xA0; $ipVal&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; / 0x1000000) );
&#xA0; &#xA0; &#xA0; &#xA0; $ipVint&#xA0;&#xA0; = $ipVal-($ipArr[0]*0x1000000); // for clarity
&#xA0; &#xA0; &#xA0; &#xA0; $ipArr[1] = ($ipVint &amp; 0xFF0000)&#xA0; &gt;&gt; 16;
&#xA0; &#xA0; &#xA0; &#xA0; $ipArr[2] = ($ipVint &amp; 0xFF00&#xA0; )&#xA0; &gt;&gt; 8;
&#xA0; &#xA0; &#xA0; &#xA0; $ipArr[3] =&#xA0; $ipVint &amp; 0xFF;
&#xA0; &#xA0; &#xA0; &#xA0; $ipDotted = implode(&apos;.&apos;, $ipArr);
?>
```



  

#



-------------------------------------------------------------------------
Question : 
var_dump((int) 010);&#xA0; //Output 8

var_dump((int) &quot;010&quot;); //output 10

First one is octal notation so the output is correct. But what about the when converting &quot;010&quot; to integer. it should be also output 8 ?
--------------------------------------------------------------------------
Answer :

Casting to an integer using (int) will always cast to the default base, which is 10.

Casting a string to a number this way does not take into account the many ways of formatting an integer value in PHP (leading zero for base 8, leading &quot;0x&quot; for base 16, leading &quot;0b&quot; for base 2). It will simply look at the first characters in a string and convert them to a base 10 integer. Leading zeroes will be stripped off because they have no meaning in numerical values, so you will end up with the decimal value 10 for (int)&quot;010&quot;.

Converting an integer value between bases using (int)010 will take into account the various ways of formatting an integer. A leading zero like in 010 means the number is in octal notation, using (int)010 will convert it to the decimal value 8 in base 10.

This is similar to how you use 0x10 to write in hexadecimal (base 16) notation. Using (int)0x10 will convert that to the base 10 decimal value 16, whereas using (int)&quot;0x10&quot; will end up with the decimal value 0: since the &quot;x&quot; is not a numerical value, anything after that will be ignored.

If you want to interpret the string &quot;010&quot; as an octal value, you need to instruct PHP to do so. intval(&quot;010&quot;, 8) will interpret the number in base 8 instead of the default base 10, and you will end up with the decimal value 8. You could also use octdec(&quot;010&quot;) to convert the octal string to the decimal value 8. Another option is to use base_convert(&quot;010&quot;, 8, 10) to explicitly convert the number &quot;010&quot; from base 8 to base 10, however this function will return the string &quot;8&quot; instead of the integer 8.

Casting a string to an integer follows the same the logic used by the intval function:

Returns the integer value of var, using the specified base for the conversion (the default is base 10).
intval allows specifying a different base as the second argument, whereas a straight cast operation does not, so using (int) will always treat a string as being in base 10.

php &gt; var_export((int) &quot;010&quot;);
10
php &gt; var_export(intval(&quot;010&quot;));
10
php &gt; var_export(intval(&quot;010&quot;, 8));
8

  

#



&quot;There is no integer division operator in PHP&quot;. But since PHP 7, there is the intdiv function.

  

#



Converting to an integer works only if the input begins with a number
(int) &quot;5txt&quot; // will output the integer 5
(int) &quot;before5txt&quot; // will output the integer 0
(int) &quot;53txt&quot; // will output the integer 53
(int) &quot;53txt534text&quot; // will output the integer 53

  

#



Be careful with using the modulo operation on big numbers, it will cast a float argument to an int and may return wrong results. For example:


```
<?php
&#xA0; &#xA0; $i = 6887129852;
&#xA0; &#xA0; echo &quot;i=$i\n&quot;;
&#xA0; &#xA0; echo &quot;i%36=&quot;.($i%36).&quot;\n&quot;;
&#xA0; &#xA0; echo &quot;alternative i%36=&quot;.($i-floor($i/36)*36).&quot;\n&quot;;
?>
```

Will output:
i=6.88713E+009
i%36=-24
alternative i%36=20

  

#

[Official documentation page](https://www.php.net/manual/en/language.types.integer.php)

**[To root](/README.md)**