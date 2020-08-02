# The ReflectionMethod class



Note that the public member $class contains the name of the class in which the method has been defined:<br><br>

```
<?php
class A {public function __construct() {}}
class B extends A {}

$method = new ReflectionMethod('B', '__construct');
echo $method->class; // prints 'A'
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.reflectionmethod.php)

**[To root](/README.md)**