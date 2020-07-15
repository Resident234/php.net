# is_string



Using is_string() on an object will always return false (even with __toString()).<br><br>

```
<?php
class B {
  public function __toString() {
    return "Instances of B() can be treated as a strings!\n";
  }
}  

$b = new B();
print($b); //Instances of B() can be treated as a strings!
print(is_string($b) ? 'true' : 'false'); //false
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-string.php)

**[To root](/README.md)**