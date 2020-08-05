# FAQ: things you need to know about namespaces



There is a way to define a namespaced constant that is a special, built-in constant, using define function and setting the third parameter case_insensitive to false:<br><br>

```
<?php
namespace foo;
define(__NAMESPACE__ . '\NULL', 10); // defines the constant NULL in the current namespace
var_dump(NULL); // will show 10
var_dump(null); // will show NULL
?>
```


  No need to specify the namespace in your call to define(), like it happens usually


```
<?php
namespace foo;
define(INI_ALL, 'bar'); // produces notice - Constant INI_ALL already defined. But:

define(__NAMESPACE__ . '\INI_ALL', 'bar'); // defines the constant INI_ALL in the current namespace
var_dump(INI_ALL); // will show string(3)"bar". Nothing unespected so far. But:

define('NULL', 10); // defines the constant NULL in the current namespace...
var_dump(NULL); // will show 10
var_dump(null); // will show NULL
?>
```


  If the parameter case_insensitive is set to true


```
<?php
namespace foo;
define (__NAMESPACE__ . '\NULL', 10, true); // produces notice - Constant null already defined
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.namespaces.faq.php)

**[To root](/README.md)**