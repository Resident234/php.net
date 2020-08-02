# get_defined_constants



Add this method to your class definition if you want an array of class constants (get_defined_constants doesn&apos;t work with class constants as Peter P said above):<br><br>

```
<?php
public function get_class_constants()
{
    $reflect = new ReflectionClass(get_class($this));
    return $reflect->getConstants());
}
?>
```
<br><br>You could also override stdObject with it so that all your classes  have this method  

---

[Official documentation page](https://www.php.net/manual/en/function.get-defined-constants.php)

**[To root](/README.md)**