# String Operators





As for me, curly braces serve good substitution for concatenation, and they are quicker to type and code looks cleaner. Remember to use double quotes (&quot; &quot;) as their content is parced by php, because in single quotes (&apos; &apos;) you&apos;ll get litaral name of variable provided:



```
<?php

 $a = &apos;12345&apos;;

// This works:
 echo &quot;qwe{$a}rty&quot;; // qwe12345rty, using braces
 echo &quot;qwe&quot; . $a . &quot;rty&quot;; // qwe12345rty, concatenation used

// Does not work:
 echo &apos;qwe{$a}rty&apos;; // qwe{$a}rty, single quotes are not parsed
 echo &quot;qwe$arty&quot;; // qwe, because $a became $arty, which is undefined

?>
```



  

#



A word of caution - the dot operator has the same precedence as + and -, which can yield unexpected results. 

Example:

&lt;php
$var = 3;

echo &quot;Result: &quot; . $var + 3;
?>
```


The above will print out &quot;3&quot; instead of &quot;Result: 6&quot;, since first the string &quot;Result3&quot; is created and this is then added to 3 yielding 3, non-empty non-numeric strings being converted to 0.

To print &quot;Result: 6&quot;, use parantheses to alter precedence:

&lt;php
$var = 3;

echo &quot;Result: &quot; . ($var + 3); 
?>
```


  

#





```
<?php 
&quot;{$str1}{$str2}{$str3}&quot;; // one concat = fast
&#xA0; $str1. $str2. $str3;&#xA0;&#xA0; // two concats = slow
?>
```

Use double quotes to concat more than two strings instead of multiple &apos;.&apos; operators.&#xA0; PHP is forced to re-concatenate with every &apos;.&apos; operator.

  

#



If you attempt to add numbers with a concatenation operator, your result will be the result of those numbers as strings.



```
<?php

echo &quot;thr&quot;.&quot;ee&quot;;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //prints the string &quot;three&quot;
echo &quot;twe&quot; . &quot;lve&quot;;&#xA0; &#xA0; &#xA0; &#xA0; //prints the string &quot;twelve&quot;
echo 1 . 2;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //prints the string &quot;12&quot;
echo 1.2;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //prints the number 1.2
echo 1+2;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //prints the number 3

?>
```



  

#



Be careful so that you don&apos;t type &quot;.&quot; instead of &quot;;&quot; at the end of a line.

It took me more than 30 minutes to debug a long script because of something like this:

&lt;?
echo &apos;a&apos;.
$c = &apos;x&apos;;
echo &apos;b&apos;;
echo &apos;c&apos;;
?>
```


The output is &quot;axbc&quot;, because of the dot on the first line.

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.string.php)

**[To root](/README.md)**