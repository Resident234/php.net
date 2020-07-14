# Comparing Objects



Note that when comparing object attributes, the comparison is recursive (at least, it is with PHP 5.2). That is, if $a-&gt;x contains an object then that will be compared with $b-&gt;x in the same manner. Be aware that this can lead to recursion errors:<br>

```
<?php
class Foo {
    public $x;
}
$a = new Foo();
$b = new Foo();
$a->x = $b;
$b->x = $a;

print_r($a == $b);
?>
```
<br>Results in:<br>PHP Fatal error:  Nesting level too deep - recursive dependency? in test.php on line 11  

#

Comparison using &lt;&gt; operators should be documented.  Between two objects, at least in PHP5.3, the comparison operation stops and returns at the first unequal property found.<br><br>

```
<?php

$o1 = new stdClass();
$o1->prop1 = 'c';
$o1->prop2 = 25;
$o1->prop3 = 201;
$o1->prop4 = 1000;

$o2 = new stdClass();
$o2->prop1 = 'c';
$o2->prop2 = 25;
$o2->prop3 = 200;
$o2->prop4 = 9999;

echo (int)($o1 &lt; $o2); // 0
echo (int)($o1 &gt; $o2); // 1

$o1->prop3 = 200;

echo (int)($o1 &lt; $o2); // 1
echo (int)($o1 &gt; $o2); // 0

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.object-comparison.php)

**[To root](/README.md)**