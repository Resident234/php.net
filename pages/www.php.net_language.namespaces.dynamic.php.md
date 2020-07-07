# Namespaces and dynamic language features





When extending a class from another namespace that should instantiate a class from within the current namespace, you need to pass on the namespace.



```
<?php // File1.php
namespace foo;
class A {
&#xA0; &#xA0; public function factory() {
&#xA0; &#xA0; &#xA0; &#xA0; return new C;
&#xA0; &#xA0; }
}
class C {
&#xA0; &#xA0; public function tell() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;foo&quot;;
&#xA0; &#xA0; }
}
?>
```




```
<?php // File2.php
namespace bar;
class B extends \foo\A {}
class C {
&#xA0; &#xA0; public function tell() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;bar&quot;;
&#xA0; &#xA0; }
}
?>
```




```
<?php
include &quot;File1.php&quot;;
include &quot;File2.php&quot;;
$b = new bar\B;
$c = $b-&gt;factory();
$c-&gt;tell(); // &quot;foo&quot; but you want &quot;bar&quot;
?>
```


You need to do it like this:

When extending a class from another namespace that should instantiate a class from within the current namespace, you need to pass on the namespace.



```
<?php // File1.php
namespace foo;
class A {
&#xA0; &#xA0; protected $namespace = __NAMESPACE__;
&#xA0; &#xA0; public function factory() {
&#xA0; &#xA0; &#xA0; &#xA0; $c = $this-&gt;namespace . &apos;\C&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; return new $c;
&#xA0; &#xA0; }
}
class C {
&#xA0; &#xA0; public function tell() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;foo&quot;;
&#xA0; &#xA0; }
}
?>
```




```
<?php // File2.php
namespace bar;
class B extends \foo\A {
&#xA0; &#xA0; protected $namespace = __NAMESPACE__;
}
class C {
&#xA0; &#xA0; public function tell() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;bar&quot;;
&#xA0; &#xA0; }
}
?>
```




```
<?php
include &quot;File1.php&quot;;
include &quot;File2.php&quot;;
$b = new bar\B;
$c = $b-&gt;factory();
$c-&gt;tell(); // &quot;bar&quot;
?>
```


(it seems that the namespace-backslashes are stripped from the source code in the preview, maybe it works in the main view. If not: fooA was written as \foo\A and barB as bar\B)

  

#



Please be aware of FQCN (Full Qualified Class Name) point.

Many people will have troubles with this:





```
<?php



// File1.php

namespace foo;



class Bar { ... }



function factory($class) {

&#xA0; &#xA0; return new $class;

}



// File2.php

$bar = \foo\factory(&apos;Bar&apos;); // Will try to instantiate \Bar, not \foo\Bar



?>
```




To fix that, and also incorporate a 2 step namespace resolution, you can check for \ as first char of $class, and if not present, build manually the FQCN:





```
<?php



// File1.php

namespace foo;



function factory($class) {

&#xA0; &#xA0; if ($class[0] != &apos;\\&apos;) {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;-&gt;&apos;;

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $class = &apos;\\&apos; . __NAMESPACE__ . &apos;\\&apos; . $class;

&#xA0; &#xA0; }



&#xA0; &#xA0; return new $class();

}



// File2.php

$bar = \foo\factory(&apos;Bar&apos;); // Will correctly instantiate \foo\Bar



$bar2 = \foo\factory(&apos;\anotherfoo\Bar&apos;); // Wil correctly instantiate \anotherfoo\Bar



?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.dynamic.php)

**[To root](/README.md)**