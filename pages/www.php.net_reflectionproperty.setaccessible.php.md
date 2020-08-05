# ReflectionProperty::setAccessible



Note that the property will only become accessible using the ReflectionProperty class. The property is still private or protected in the class instances.<br><br>

```
<?php
class MyClass {
     private $myProperty = true;
}

$class = new ReflectionClass("MyClass");
$property = $class->getProperty("myProperty");
$property->setAccessible(true);

$obj = new MyClass();
echo $property->getValue($obj); // Works
echo $obj->myProperty; // Doesn't work (error)
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/reflectionproperty.setaccessible.php)

**[To root](/README.md)**