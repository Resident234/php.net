# ReflectionClass::getInterfaceNames



It seems the interface names are actually listed in a defined order:<br>- "extends" takes precedence over "implements" (i.e. first will be interfaces from (implemented in) the parent class (if any), then interfaces implemented in the class itself)<br>- when multiple interfaces are implemented at one time/level, it can be:<br>+ from an "implements" : they&apos;re listed in the defined order<br>+ from an "extends" (a class extends another class which implements multiple interfaces; or an interface extends multiple interfaces) : they&apos;re listed in REVERSE order<br><br>

```
<?php
interface Foo {}
interface Bar {}
interface Other {}
interface Foobar extends Foo, Bar {}
interface Barfoo extends Bar, Foo {}

class Test1 implements Foo, Bar {}
class Test2 implements Bar, Foo {}

class Test3 extends Test1 {}
class Test4 extends Test2 {}

class Test5 extends Test1 implements Other {}

class Test6 implements Foobar, Other {}
class TestO implements Other {}
class Test7 extends TestO implements Barfoo {}

$r=new ReflectionClass(&apos;Test1&apos;);
print_r($r-&gt;getInterfaceNames()); // Foo, Bar

$r=new ReflectionClass(&apos;Test2&apos;);
print_r($r-&gt;getInterfaceNames()); // Bar, Foo

$r=new ReflectionClass(&apos;Test3&apos;);
print_r($r-&gt;getInterfaceNames()); // Bar, Foo

$r=new ReflectionClass(&apos;Test4&apos;);
print_r($r-&gt;getInterfaceNames()); // Foo, Bar

$r=new ReflectionClass(&apos;Test5&apos;);
print_r($r-&gt;getInterfaceNames()); // Bar, Foo, Other

$r=new ReflectionClass(&apos;Test6&apos;);
print_r($r-&gt;getInterfaceNames()); // Foobar, Bar, Foo, Other

$r=new ReflectionClass(&apos;Test7&apos;);
print_r($r-&gt;getInterfaceNames()); // Other, Barfoo, Foo, Bar
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getinterfacenames.php)

**[To root](/README.md)**