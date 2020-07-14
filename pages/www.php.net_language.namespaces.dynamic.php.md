# Namespaces and dynamic language features



When extending a class from another namespace that should instantiate a class from within the current namespace, you need to pass on the namespace.<br><br>

```
<?php // File1.php
namespace foo;
class A {
    public function factory() {
        return new C;
    }
}
class C {
    public function tell() {
        echo "foo";
    }
}
?>
```




```
<?php // File2.php
namespace bar;
class B extends \foo\A {}
class C {
    public function tell() {
        echo "bar";
    }
}
?>
```




```
<?php
include "File1.php";
include "File2.php";
$b = new bar\B;
$c = $b->factory();
$c->tell(); // "foo" but you want "bar"
?>
```


You need to do it like this:

When extending a class from another namespace that should instantiate a class from within the current namespace, you need to pass on the namespace.



```
<?php // File1.php
namespace foo;
class A {
    protected $namespace = __NAMESPACE__;
    public function factory() {
        $c = $this->namespace . '\C';
        return new $c;
    }
}
class C {
    public function tell() {
        echo "foo";
    }
}
?>
```




```
<?php // File2.php
namespace bar;
class B extends \foo\A {
    protected $namespace = __NAMESPACE__;
}
class C {
    public function tell() {
        echo "bar";
    }
}
?>
```




```
<?php
include "File1.php";
include "File2.php";
$b = new bar\B;
$c = $b->factory();
$c->tell(); // "bar"
?>
```
<br><br>(it seems that the namespace-backslashes are stripped from the source code in the preview, maybe it works in the main view. If not: fooA was written as \foo\A and barB as bar\B)  

#

Please be aware of FQCN (Full Qualified Class Name) point.<br>Many people will have troubles with this:<br><br>

```
<?php

// File1.php
namespace foo;

class Bar { ... }

function factory($class) {
    return new $class;
}

// File2.php
$bar = \foo\factory('Bar'); // Will try to instantiate \Bar, not \foo\Bar

?>
```


To fix that, and also incorporate a 2 step namespace resolution, you can check for \ as first char of $class, and if not present, build manually the FQCN:



```
<?php

// File1.php
namespace foo;

function factory($class) {
    if ($class[0] != '\\') {
        echo '->';
         $class = '\\' . __NAMESPACE__ . '\\' . $class;
    }

    return new $class();
}

// File2.php
$bar = \foo\factory('Bar'); // Will correctly instantiate \foo\Bar

$bar2 = \foo\factory('\anotherfoo\Bar'); // Wil correctly instantiate \anotherfoo\Bar

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.dynamic.php)

**[To root](/README.md)**