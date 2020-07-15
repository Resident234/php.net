# ReflectionParameter::getClass



The method returns ReflectionClass object of parameter type class or NULL if none.<br><br>

```
<?php

class A {
    function b(B $c, array $d, $e) {
    }
}
class B {
}

$refl = new ReflectionClass('A');
$par = $refl->getMethod('b')->getParameters();

var_dump($par[0]->getClass()->getName());  // outputs B
var_dump($par[1]->getClass());  // note that array type outputs NULL
var_dump($par[2]->getClass());  // outputs NULL

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionparameter.getclass.php)

**[To root](/README.md)**