# ReflectionClass::getConstants



If you want to return the constants defined inside a class then you can also define an internal method as follows:<br><br>

```
<?php
class myClass {
    const NONE = 0;
    const REQUEST = 100;
    const AUTH = 101;

    // others...

    static function getConstants() {
        $oClass = new ReflectionClass(__CLASS__);
        return $oClass->getConstants();
    }
}
?>
```
  

#

You can pass $this as class for the ReflectionClass. __CLASS__ won&apos;t help if you extend the original class, because it is a magic constant based on the file itself.<br><br>

```
<?php 

class Example {
  const TYPE_A = 1;
  const TYPE_B = 'hello';

  public function getConstants()
  {
    $reflectionClass = new ReflectionClass($this);
    return $reflectionClass->getConstants();
  }
}

$example = new Example();
var_dump($example->getConstants());

// Result:
array ( size = 2)
  'TYPE_A' => int 1
  'TYPE_B' => (string) 'hello'?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getconstants.php)

**[To root](/README.md)**