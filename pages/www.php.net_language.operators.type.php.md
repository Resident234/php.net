# Type Operators



Posting this so the word typeof appears on this page, so that this page will show up when you google &apos;php typeof&apos;.  ...yeah, former Java user.  

#

You are also able to compare 2 objects using instanceOf. In that case, instanceOf will compare the types of both objects. That is sometimes very useful:<br><br>

```
<?php

class A { }
class B { }

$a = new A;
$b = new B;
$a2 = new A;

echo $a instanceOf $a; // true
echo $a instanceOf $b; // false
echo $a instanceOf $a2; // true

?>
```
  

#

Checking an object is not an instance of a class, example #3 uses extraneous parentheses.<br><br>

```
<?php
var_dump(!($a instanceof stdClass));
?>
```


Because instanceof has higher operator precedence than ! you can just do



```
<?php
var_dump( ! $a instanceof stdClass );
?>
```
  

#

I don&apos;t see any mention of "namespaces" on this page so I thought I would chime in. The instanceof operator takes FQCN as second operator when you pass it as string and not a simple class name. It will not resolve it even if you have a `use MyNamespace\Bar;` at the top level. Here is what I am trying to say:<br><br>## testinclude.php ##<br>

```
<?php
namespace Bar1;
{
class Foo1{ }
}
namespace Bar2;
{
class Foo2{ }
}
?>
```

## test.php ##


```
<?php
include(&apos;testinclude.php&apos;);
use Bar1\Foo1 as Foo;
$foo1 = new Foo(); $className = &apos;Bar1\Foo1&apos;;
var_dump($foo1 instanceof Bar1\Foo1);
var_dump($foo1 instanceof $className);
$className = &apos;Foo&apos;;
var_dump($foo1 instanceof $className);
use Bar2\Foo2;
$foo2 = new Foo2(); $className = &apos;Bar2\Foo2&apos;;
var_dump($foo2 instanceof Bar2\Foo2);
var_dump($foo2 instanceof $className);
$className = &apos;Foo2&apos;;
var_dump($foo2 instanceof $className);
?>
```
<br>## stdout ##<br>bool(true)<br>bool(true)<br>bool(false)<br>bool(true)<br>bool(true)<br>bool(false)  

#

You can use "self" to reference to the current class:<br><br>

```
<?php
class myclass {
    function mymethod($otherObject) {
        if ($otherObject instanceof self) {
            $otherObject-&gt;mymethod(null);
        }
        return &apos;works!&apos;;
    }
}

$a = new myclass();
print $a-&gt;mymethod($a);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.type.php)

**[To root](/README.md)**