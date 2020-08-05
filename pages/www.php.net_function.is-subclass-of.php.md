# is_subclass_of



is_subclass_of() works also with classes between the class of obj and the superclass.<br><br>example:<br>

```
<?php
class A {};
class B extends A {};
class C extends B {};

$foo=new C();
echo ((is_subclass_of($foo,'A')) ? 'true' : 'false');
?>
```
<br><br>echoes &apos;true&apos; .  

---

[Official documentation page](https://www.php.net/manual/en/function.is-subclass-of.php)

**[To root](/README.md)**