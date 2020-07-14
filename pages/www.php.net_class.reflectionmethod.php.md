# The ReflectionMethod class



Note that the public member $class contains the name of the class in which the method has been defined:<br><br>

```
<?php
class A {public function __construct() {}}
class B extends A {}

$method = new ReflectionMethod(&apos;B&apos;, &apos;__construct&apos;);
echo $method-&gt;class; // prints &apos;A&apos;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.reflectionmethod.php)

**[To root](/README.md)**