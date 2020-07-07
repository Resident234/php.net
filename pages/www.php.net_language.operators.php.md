# Operators





of course this should be clear, but i think it has to be mentioned espacially:

AND is not the same like &amp;&amp;

for example:



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

&apos;cause || has got a higher priority than and, but less than &amp;&amp;

of course, using always [ &amp;&amp; and || ] or [ AND and OR ] would be okay, but than you should at least respect the following:



```
<?php $a = $b &amp;&amp; $c; ?>
```



```
<?php $a = $b AND $c; ?>
```


the first code will set $a to the result of the comparison $b with $c, both have to be true, while the second code line will set $a like $b and THAN - after that - compare the success of this with the value of $c

maybe usefull for some tricky coding and helpfull to prevent bugs :D

greetz, Warhog

  

#



Other Language books&apos; operator precedence section usually include &quot;(&quot; and &quot;)&quot; - with exception of a Perl book that I have. (In PHP &quot;{&quot; and &quot;}&quot; should also be considered also). However, PHP Manual is not listed &quot;(&quot; and &quot;)&quot; in precedence list. It looks like &quot;(&quot; and &quot;)&quot; has higher precedence as it should be.



Note: If you write following code, you would need &quot;()&quot; to get expected value.





```
<?php

$bar = true;

$str = &quot;TEST&quot;. ($bar ? &apos;true&apos; : &apos;false&apos;) .&quot;TEST&quot;;

?>
```




Without &quot;(&quot; and &quot;)&quot; you will get only &quot;true&quot; in $str. 

(PHP4.0.4pl1/Apache DSO/Linux, PHP4.0.5RC1/Apache DSO/W2K Server)

It&apos;s due to precedence, probably.

  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.php)

**[To root](/README.md)**