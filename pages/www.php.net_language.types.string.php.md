# Strings



I&apos;ve been a PHP programmer for a decade, and I&apos;ve always been using the "single-quoted literal" and "period-concatenation" method of string creation. But I wanted to answer the performance question once and for all, using sufficient numbers of iterations and a modern PHP version. For my test, I used:<br><br>php -v
PHP 7.0.12 (cli) (built: Oct 14 2016 09:56:59) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies

------ Results: -------

* 100 million iterations:

$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n;
63608ms (34.7% slower)

$outstr = "literal$n$data$int$data$float$n";
47218ms (fastest)

$outstr =&lt;&lt;&lt;EOS
literal$n$data$int$data$float$n
EOS;
47992ms (1.64% slower)

$outstr = sprintf(&apos;literal%s%s%d%s%f%s&apos;, $n, $data, $int, $data, $float, $n);
76629ms (62.3% slower)

$outstr = sprintf(&apos;literal%s%5$s%2$d%3$s%4$f%s&apos;, $n, $int, $data, $float, $data, $n);
96260ms (103.9% slower)

* 10 million iterations (test adapted to see which of the two fastest methods were faster at adding a newline; either the PHP_EOL literal, or the \n string expansion):

$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n;
6228ms (reference for single-quoted without newline)

$outstr = "literal$n$data$int$data$float$n";
4653ms (reference for double-quoted without newline)

$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n . PHP_EOL;
6630ms (35.3% slower than double-quoted with \n newline)

$outstr = "literal$n$data$int$data$float$n\n";
4899ms (fastest at newlines)

* 100 million iterations (a test intended to see which one of the two ${var} and {$var} double-quote styles is faster):

$outstr = &apos;literal&apos; . $n . $data . $int . $data . $float . $n;
67048ms (38.2% slower)

$outstr = "literal$n$data$int$data$float$n";
49058ms (1.15% slower)

$outstr = "literal{$n}{$data}{$int}{$data}{$float}{$n}"
49221ms (1.49% slower)

$outstr = "literal${n}${data}${int}${data}${float}${n}"
48500ms (fastest; the differences are small but this held true across multiple runs of the test, and this was always the fastest variable encapsulation style)

* 1 BILLION iterations (testing a completely literal string with nothing to parse in it):

$outstr = &apos;literal string testing&apos;;
23852ms (fastest)

$outstr = "literal string testing";
24222ms (1.55% slower)

It blows my mind. The double-quoted strings "which look so $slow since they have to parse everything for \n backslashes and $dollar signs to do variable expansion", turned out to be the FASTEST string concatenation method in PHP - PERIOD!

Single-quotes are only faster if your string is completely literal (with nothing to parse in it and nothing to concatenate), but the margin is very tiny and doesn&apos;t matter.

So the "highest code performance" style rules are:

1. Always use double-quoted strings for concatenation.

2. Put your variables in "This is a {$variable} notation", because it&apos;s the fastest method which still allows complex expansions like "This {$var[&apos;foo&apos;]} is {$obj-&gt;awesome()}!". You cannot do that with the "${var}" style.

3. Feel free to use single-quoted strings for TOTALLY literal strings such as array keys/values, variable values, etc, since they are a TINY bit faster when you want literal non-parsed strings. But I had to do 1 billion iterations to find a 1.55% measurable difference. So the only real reason I&apos;d consider using single-quoted strings for my literals is for code cleanliness, to make it super clear that the string is literal.

4. If you think another method such as sprintf() or &apos;this&apos;.$var.&apos;style&apos; is more readable, and you don&apos;t care about maximizing performance, then feel free to use whatever concatenation method you prefer!?>
```
  

#

The documentation does not mention, but a closing semicolon at the end of the heredoc is actually interpreted as a real semicolon, and as such, sometimes leads to syntax errors.<br><br>This works:<br><br>

```
<?php
$foo = &lt;&lt;&lt;END
abcd
END;
?>
```


This does not:



```
<?php
foo(&lt;&lt;&lt;END
abcd
END;
);
// syntax error, unexpected &apos;;&apos;
?>
```


Without semicolon, it works fine:



```
<?php
foo(&lt;&lt;&lt;END
abcd
END
);
?>
```
  

#

md5(&apos;240610708&apos;) == md5(&apos;QNKCDZO&apos;)<br><br>This comparison is true because both md5() hashes start &apos;0e&apos; so PHP type juggling understands these strings to be scientific notation.  By definition, zero raised to any power is zero.  

#

You can use string like array of char (like C)<br><br>$a = "String array test";<br><br>var_dump($a); <br>// Return string(17) "String array test"<br><br>var_dump($a[0]); <br>// Return string(1) "S"<br><br>// -- With array cast --<br>var_dump((array) $a); <br>// Return array(1) { [0]=&gt; string(17) "String array test"}<br><br>var_dump((array) $a[0]); <br>// Return string(17) "S"<br><br>- Norihiori  

#

You can use the complex syntax to put the value of both object properties AND object methods inside a string.  For example...<br>

```
<?php
class Test {
    public $one = 1;
    public function two() {
        return 2;
    }
}
$test = new Test();
echo "foo {$test-&gt;one} bar {$test-&gt;two()}";
?>
```

Will output "foo 1 bar 2".

However, you cannot do this for all values in your namespace.  Class constants and static properties/methods will not work because the complex syntax looks for the &apos;

```
<?php<br>class Test {<br>    public $one = 1;<br>    public function two() {<br>        return 2;<br>    }<br>}<br>$test = new Test();<br>echo "foo {$test-&gt;one} bar {$test-&gt;two()}";<br>?>
```
<br>Will output "foo 1 bar 2".<br><br>However, you cannot do this for all values in your namespace.  Class constants and static properties/methods will not work because the complex syntax looks for the &apos;$&apos;.<br>

```
<?php<br>class Test {<br>    const ONE = 1;<br>}<br>echo "foo {Test::ONE} bar";<br>?>
```
apos;.


```
<?php
class Test {
    const ONE = 1;
}
echo "foo {Test::ONE} bar";
?>
```
<br>This will output "foo {Test::one} bar".  Constants and static properties require you to break up the string.  

#

easy transparent solution for using constants in the heredoc format:<br>DEFINE(&apos;TEST&apos;,&apos;TEST STRING&apos;);<br><br>$const = get_defined_constants();<br><br>echo &lt;&lt;&lt;END<br>{$const[&apos;TEST&apos;]}<br>END;<br><br>Result:<br>TEST STRING  

#

Here is an easy hack to allow double-quoted strings and heredocs to contain arbitrary expressions in curly braces syntax, including constants and other function calls:<br><br>

```
<?php

// Hack declaration
function _expr($v) { return $v; }
$_expr = &apos;_expr&apos;;

// Our playground
define(&apos;qwe&apos;, &apos;asd&apos;);
define(&apos;zxc&apos;, 5);

$a=3;
$b=4;

function c($a, $b) { return $a+$b; }

// Usage
echo "pre {$_expr(1+2)} post\n"; // outputs &apos;pre 3 post&apos;
echo "pre {$_expr(qwe)} post\n"; // outputs &apos;pre asd post&apos;
echo "pre {$_expr(c($a, $b)+zxc*2)} post\n"; // outputs &apos;pre 17 post&apos;

// General syntax is {$_expr(...)}
?>
```
  

#

To save Your mind don&apos;t read previous comments about dates  ;)<br><br>When both strings can be converted to the numerics (in ("$a" &gt; "$b") test) then resulted numerics are used, else FULL strings are compared char-by-char:<br><br>

```
<?php
var_dump(&apos;1.22&apos; &gt; &apos;01.23&apos;); // bool(false)
var_dump(&apos;1.22.00&apos; &gt; &apos;01.23.00&apos;); // bool(true)
var_dump(&apos;1-22-00&apos; &gt; &apos;01-23-00&apos;); // bool(true)
var_dump((float)&apos;1.22.00&apos; &gt; (float)&apos;01.23.00&apos;); // bool(false)
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.string.php)

**[To root](/README.md)**