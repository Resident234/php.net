# constant



The constant name can be an empty string.<br><br>Code:<br><br>define("", "foo");<br>echo constant("");<br><br>Output:<br><br>foo  

#

If you are referencing class constant (either using namespaces or not, because one day you may want to start using them), you&apos;ll have the least headaches when doing it like this:<br><br>

```
<?php
class Foo {
    const BAR = 42;
}
?>
```



```
<?php
namespace Baz;
use \Foo as F;

echo constant(F::class.&apos;::BAR&apos;);
?>
```
<br><br>since F::class will be dereferenced to whatever namespace shortcuts you are using (and those are way easier to refactor for IDE than just plain strings with hardcoded namespaces in string literals)  

#

As of PHP 5.4.6 constant() pays no attention to any namespace aliases that might be defined in the file in which it&apos;s used. I.e. constant() always behaves as if it is called from the global namespace. This means that the following will not work:<br><br>

```
<?php
class Foo {
    const BAR = 42;
}
?>
```




```
<?php
namespace Baz;

use \Foo as F;

echo constant(&apos;F::BAR&apos;);
?>
```
<br><br>However, calling constant(&apos;Foo::BAR&apos;) will work as expected.  

#

[Official documentation page](https://www.php.net/manual/en/function.constant.php)

**[To root](/README.md)**