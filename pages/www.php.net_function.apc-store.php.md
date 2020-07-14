# apc_store



if you want to store array of objects in apc use ArrayObject wrapper (PHP5).<br><br>

```
<?php
$objs = array();
$objs[] = new TestClass();
$objs[] = new TestClass();
$objs[] = new TestClass();

//Doesn't work
apc_store('objs',$objs,60);
$tmp = apc_fetch('objs'); 
print_r($tmp);

//Works
apc_store('objs',new ArrayObject($objs),60);
$tmp = apc_fetch('objs'); 
print_r($tmp->getArrayCopy());

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.apc-store.php)

**[To root](/README.md)**