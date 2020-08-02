# New features



It is also possible ( in 5.6.0alpha ) to typehint the ...-operator<br><br>function foo (stdclass ... $inbound) {<br>   var_dump($inbound);<br>}<br><br>// ok:<br>foo( (object)[&apos;foo&apos; =&gt; &apos;bar&apos;], (object)[&apos;bar&apos; =&gt; &apos;foo&apos;] );<br><br>// fails:<br>foo( 1, 2, 3, 4 );  

---

Remember, that<br><br>    ($a ** $b) ** $c === $a ** ($b * $c)<br><br>Thats why exponent operator** is RIGHT associative.  

---

Note the order of operations in that exponentiation operator, as it was opposite of what my first expectation was:<br><br>

```
<?php
// what I had expected, 
// evaluating left to right, 
// since no parens were used to guide the order of operations
2 ** 3 ** 2 == 64; // (2 ** 3) ** 2 = 8 ** 2 = 64

// the given example, 
// which appears to evaluate right to left
2 ** 3 ** 2 == 512; // 2 ** (3 ** 2) = 2 ** 9 = 512
?>
```
  

---



```
<?php

function array_zip(...$arrays) {
    return array_merge(...array_map(NULL, ...$arrays));
}

$a = array(1, 4, 7);
$b = array(2, 5, 8);
$c = array(3, 6, 9);

var_dump(implode(', ', array_zip($a, $b, $c)));

// Output
string(25) "1, 2, 3, 4, 5, 6, 7, 8, 9"?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/migration56.new-features.php)

**[To root](/README.md)**