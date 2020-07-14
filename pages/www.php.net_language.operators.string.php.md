# String Operators



As for me, curly braces serve good substitution for concatenation, and they are quicker to type and code looks cleaner. Remember to use double quotes (" ") as their content is parced by php, because in single quotes (&apos; &apos;) you&apos;ll get litaral name of variable provided:<br><br>

```
<?php

 $a = '12345';

// This works:
 echo "qwe{$a}rty"; // qwe12345rty, using braces
 echo "qwe" . $a . "rty"; // qwe12345rty, concatenation used

// Does not work:
 echo 'qwe{$a}rty'; // qwe{$a}rty, single quotes are not parsed
 echo "qwe$arty"; // qwe, because $a became $arty, which is undefined

?>
```
  

#

A word of caution - the dot operator has the same precedence as + and -, which can yield unexpected results. <br><br>Example:<br><br>&lt;php<br>$var = 3;<br><br>echo "Result: " . $var + 3;<br>?>
```
<br><br>The above will print out "3" instead of "Result: 6", since first the string "Result3" is created and this is then added to 3 yielding 3, non-empty non-numeric strings being converted to 0.<br><br>To print "Result: 6", use parantheses to alter precedence:<br><br>&lt;php<br>$var = 3;<br><br>echo "Result: " . ($var + 3); <br>?>
```
  

#



```
<?php 
"{$str1}{$str2}{$str3}"; // one concat = fast
  $str1. $str2. $str3;   // two concats = slow
?>
```
<br>Use double quotes to concat more than two strings instead of multiple &apos;.&apos; operators.  PHP is forced to re-concatenate with every &apos;.&apos; operator.  

#

If you attempt to add numbers with a concatenation operator, your result will be the result of those numbers as strings.<br><br>

```
<?php

echo "thr"."ee";           //prints the string "three"
echo "twe" . "lve";        //prints the string "twelve"
echo 1 . 2;                //prints the string "12"
echo 1.2;                  //prints the number 1.2
echo 1+2;                  //prints the number 3

?>
```
  

#

Be careful so that you don&apos;t type "." instead of ";" at the end of a line.<br><br>It took me more than 30 minutes to debug a long script because of something like this:<br><br>&lt;?<br>echo &apos;a&apos;.<br>$c = &apos;x&apos;;<br>echo &apos;b&apos;;<br>echo &apos;c&apos;;<br>?>
```
<br><br>The output is "axbc", because of the dot on the first line.  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.string.php)

**[To root](/README.md)**