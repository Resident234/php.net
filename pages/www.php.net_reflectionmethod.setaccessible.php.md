# ReflectionMethod::setAccessible



This is handy for accessing private methods but remember that things are normally private for a reason! Unit Testing is one (debatable) use case for this.<br><br>Example:<br>

```
<?php
class Foo {
  private function myPrivateMethod() {
    return 7;
  }
}

$method = new ReflectionMethod(&apos;Foo&apos;, &apos;myPrivateMethod&apos;);
$method-&gt;setAccessible(true);
 
echo $method-&gt;invoke(new Foo);
// echos "7"
?>
```
<br><br>This works nicely with PHPUnit: http://php.net/manual/en/reflectionmethod.setaccessible.php  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.setaccessible.php)

**[To root](/README.md)**