# The ReflectionClass class



To reflect on a namespaced class in PHP 5.3, you must always specify the fully qualified name of the class - even if you&apos;ve aliased the containing namespace using a "use" statement.<br><br>So instead of:<br><br>

```
<?php
use App\Core as Core;
$oReflectionClass = new ReflectionClass('Core\Singleton');
?>
```


You would type:



```
<?php
use App\Core as Core;
$oReflectionClass = new ReflectionClass('App\Core\Singleton');
?>
```
  

---

Reflecting an alias will give you a reflection of the resolved class.<br><br>

```
<?php

class X {
    
}

class_alias('X','Y');
class_alias('Y','Z');
$z = new ReflectionClass('Z');
echo $z->getName(); // X

?>
```
  

---

Unserialized reflection class cause error.<br><br>

```
<?php
/**
 * abc
 */
class a{}

$ref = new ReflectionClass('a');
$ref = unserialize(serialize($ref));
var_dump($ref);
var_dump($ref->getDocComment());

// object(ReflectionClass)#2 (1) {
//   ["name"]=>
//   string(1) "a"
// }
// PHP Fatal error:  ReflectionClass::getDocComment(): Internal error: Failed to retrieve the reflection object
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.reflectionclass.php)

**[To root](/README.md)**