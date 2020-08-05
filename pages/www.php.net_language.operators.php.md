# Operators



of course this should be clear, but i think it has to be mentioned espacially:<br><br>AND is not the same like &amp;&amp;<br><br>for example:<br><br>

```
<?php $a &amp;&amp; $b || $c; ?>
```

is not the same like


```
<?php $a AND $b || $c; ?>
```


the first thing is
(a and b) or c

the second
a and (b or c)

'cause || has got a higher priority than and, but less than &amp;&amp;

of course, using always [ &amp;&amp; and || ] or [ AND and OR ] would be okay, but than you should at least respect the following:



```
<?php $a = $b &amp;&amp; $c; ?>
```



```
<?php $a = $b AND $c; ?>
```
<br><br>the first code will set $a to the result of the comparison $b with $c, both have to be true, while the second code line will set $a like $b and THAN - after that - compare the success of this with the value of $c<br><br>maybe usefull for some tricky coding and helpfull to prevent bugs :D<br><br>greetz, Warhog  

---

Other Language books&apos; operator precedence section usually include "(" and ")" - with exception of a Perl book that I have. (In PHP "{" and "}" should also be considered also). However, PHP Manual is not listed "(" and ")" in precedence list. It looks like "(" and ")" has higher precedence as it should be.<br><br>Note: If you write following code, you would need "()" to get expected value.<br><br>

```
<?php
$bar = true;
$str = "TEST". ($bar ? 'true' : 'false') ."TEST";
?>
```
<br><br>Without "(" and ")" you will get only "true" in $str. <br>(PHP4.0.4pl1/Apache DSO/Linux, PHP4.0.5RC1/Apache DSO/W2K Server)<br>It&apos;s due to precedence, probably.  

---

[Official documentation page](https://www.php.net/manual/en/language.operators.php)

**[To root](/README.md)**