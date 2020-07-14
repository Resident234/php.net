# The ReflectionClass class



To reflect on a namespaced class in PHP 5.3, you must always specify the fully qualified name of the class - even if you&apos;ve aliased the containing namespace using a "use" statement.<br><br>So instead of:<br><br>

```
<?php
use App\Core as Core;
$oReflectionClass = new ReflectionClass(&apos;Core\Singleton&apos;);
?>
```


You would type:



```
<?php
use App\Core as Core;
$oReflectionClass = new ReflectionClass(&apos;App\Core\Singleton&apos;);
?>
```
  

#

Reflecting an alias will give you a reflection of the resolved class.<br><br>

```
<?php

class X {
    
}

class_alias(&apos;X&apos;,&apos;Y&apos;);
class_alias(&apos;Y&apos;,&apos;Z&apos;);
$z = new ReflectionClass(&apos;Z&apos;);
echo $z-&gt;getName(); // X

?>
```
  

#

Unserialized reflection class cause error.<br><br>

```
<?php
/**
 * abc
 */
class a{}

$ref = new ReflectionClass(&apos;a&apos;);
$ref = unserialize(serialize($ref));
var_dump($ref);
var_dump($ref-&gt;getDocComment());

// object(ReflectionClass)#2 (1) {
//   ["name"]=&gt;
//   string(1) "a"
// }
// PHP Fatal error:  ReflectionClass::getDocComment(): Internal error: Failed to retrieve the reflection object
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.reflectionclass.php)

**[To root](/README.md)**