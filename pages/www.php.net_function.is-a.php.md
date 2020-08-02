# is_a



Please note that you have to fully qualify the class name in the second parameter. <br><br>A use statement will not resolve namespace dependencies in that is_a() function. <br><br>

```
<?php 
namespace foo\bar;

class A {};
class B extends A {};
?>
```




```
<?php
namespace har\var;

use foo\bar\A;
$foo = new foo\bar\B();

is_a($foo, 'A'); // returns false;
is_a($foo, 'foo\bar\A'); // returns true;
?>
```
<br><br>Just adding that note here because all examples are without namespaces.  

---

Be careful! Starting in PHP 5.3.7 the behavior of is_a() has changed slightly: when calling is_a() with a first argument that is not an object, __autoload() is triggered!<br><br>In practice, this means that calling is_a(&apos;23&apos;, &apos;User&apos;); will trigger __autoload() on "23". Previously, the above statement simply returned &apos;false&apos;.<br><br>More info can be found here:<br>https://bugs.php.net/bug.php?id=55475<br><br>Whether this change is considered a bug and whether it will be reverted or kept in future versions is yet to be determined, but nevertheless it is how it is, for now...  

---

At least in PHP 5.1.6 this works as well with Interfaces.<br><br>

```
<?php
interface test {
  public function A();
}

class TestImplementor implements test {
  public function A () {
    print "A";
  }
}

$testImpl = new TestImplementor();

var_dump(is_a($testImpl,'test'));
?>
```
<br><br>will return true  

---

[Official documentation page](https://www.php.net/manual/en/function.is-a.php)

**[To root](/README.md)**