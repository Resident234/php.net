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

$r=new ReflectionClass('Test1');
print_r($r->getInterfaceNames()); // Foo, Bar

$r=new ReflectionClass('Test2');
print_r($r->getInterfaceNames()); // Bar, Foo

$r=new ReflectionClass('Test3');
print_r($r->getInterfaceNames()); // Bar, Foo

$r=new ReflectionClass('Test4');
print_r($r->getInterfaceNames()); // Foo, Bar

$r=new ReflectionClass('Test5');
print_r($r->getInterfaceNames()); // Bar, Foo, Other

$r=new ReflectionClass('Test6');
print_r($r->getInterfaceNames()); // Foobar, Bar, Foo, Other

$r=new ReflectionClass('Test7');
print_r($r->getInterfaceNames()); // Other, Barfoo, Foo, Bar
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getinterfacenames.php)

**[To root](/README.md)**