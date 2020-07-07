# get_defined_constants





Add this method to your class definition if you want an array of class constants (get_defined_constants doesn&apos;t work with class constants as Peter P said above):



```
<?php
public function get_class_constants()
{
&#xA0; &#xA0; $reflect = new ReflectionClass(get_class($this));
&#xA0; &#xA0; return $reflect-&gt;getConstants());
}
?>
```


You could also override stdObject with it so that all your classes&#xA0; have this method

  

#

[Official documentation page](https://www.php.net/manual/en/function.get-defined-constants.php)

**[To root](/README.md)**