# FAQ: things you need to know about namespaces





There is a way to define a namespaced constant that is a special, built-in constant, using define function and setting the third parameter case_insensitive to false:



```
<?php
namespace foo;
define(__NAMESPACE__ . &apos;\NULL&apos;, 10); // defines the constant NULL in the current namespace
var_dump(NULL); // will show 10
var_dump(null); // will show NULL
?>
```


&#xA0; No need to specify the namespace in your call to define(), like it happens usually


```
<?php
namespace foo;
define(INI_ALL, &apos;bar&apos;); // produces notice - Constant INI_ALL already defined. But:

define(__NAMESPACE__ . &apos;\INI_ALL&apos;, &apos;bar&apos;); // defines the constant INI_ALL in the current namespace
var_dump(INI_ALL); // will show string(3)&quot;bar&quot;. Nothing unespected so far. But:

define(&apos;NULL&apos;, 10); // defines the constant NULL in the current namespace...
var_dump(NULL); // will show 10
var_dump(null); // will show NULL
?>
```


&#xA0; If the parameter case_insensitive is set to true


```
<?php
namespace foo;
define (__NAMESPACE__ . &apos;\NULL&apos;, 10, true); // produces notice - Constant null already defined
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.faq.php)

**[To root](/README.md)**