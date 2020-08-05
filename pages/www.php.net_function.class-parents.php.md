# class_parents





```
<?php
class foo {}
class bar extends foo {}
class baz extends bar {}

print_r(class_parents(new baz));
?>
```
<br><br>Will output:<br>Array<br>(<br>    [bar] =&gt; bar<br>    [foo] =&gt; foo<br>)  

---

[Official documentation page](https://www.php.net/manual/en/function.class-parents.php)

**[To root](/README.md)**