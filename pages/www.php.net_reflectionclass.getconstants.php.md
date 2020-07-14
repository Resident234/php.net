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
        return $oClass-&gt;getConstants();
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
  const TYPE_B = &apos;hello&apos;;

  public function getConstants()
  {
    $reflectionClass = new ReflectionClass($this);
    return $reflectionClass-&gt;getConstants();
  }
}

$example = new Example();
var_dump($example-&gt;getConstants());

// Result:
array ( size = 2)
  &apos;TYPE_A&apos; =&gt; int 1
  &apos;TYPE_B&apos; =&gt; (string) &apos;hello&apos;?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getconstants.php)

**[To root](/README.md)**