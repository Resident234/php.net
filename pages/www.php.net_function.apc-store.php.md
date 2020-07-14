# apc_store



if you want to store array of objects in apc use ArrayObject wrapper (PHP5).<br><br>

```
<?php
$objs = array();
$objs[] = new TestClass();
$objs[] = new TestClass();
$objs[] = new TestClass();

//Doesn&apos;t work
apc_store(&apos;objs&apos;,$objs,60);
$tmp = apc_fetch(&apos;objs&apos;); 
print_r($tmp);

//Works
apc_store(&apos;objs&apos;,new ArrayObject($objs),60);
$tmp = apc_fetch(&apos;objs&apos;); 
print_r($tmp-&gt;getArrayCopy());

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.apc-store.php)

**[To root](/README.md)**