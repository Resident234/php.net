# Operator Precedence



Watch out for the difference of priority between &apos;and vs &amp;&amp;&apos; or &apos;|| vs or&apos;:<br>

```
<?php
$bool = true &amp;&amp; false;
var_dump($bool); // false, that's expected

$bool = true and false;
var_dump($bool); // true, ouch!
?>
```
<br>Because &apos;and/or&apos; have lower priority than &apos;=&apos; but &apos;||/&amp;&amp;&apos; have higher.  

---

Beware the unusual order of bit-wise operators and comparison operators, this has often lead to bugs in my experience. For instance:<br><br>

```
<?php if ( $flags &amp; MASK  == 1) do_something(); ?>
```


will not do what you might expect from other languages. Use



```
<?php if (($flags &amp; MASK) == 1) do_something(); ?>
```
<br><br>in PHP instead.  

---

If you&apos;ve come here looking for a full list of PHP operators, take note that the table here is *not* complete. There are some additional operators (or operator-ish punctuation tokens) that are not included here, such as "-&gt;", "::", and "...".<br><br>For a really comprehensive list, take a look at the "List of Parser Tokens" page: http://php.net/manual/en/tokens.php  

---

[Official documentation page](https://www.php.net/manual/en/language.operators.precedence.php)

**[To root](/README.md)**