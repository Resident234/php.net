# namespace keyword and __NAMESPACE__ constant



Just in case you wonder what the practical use of the namespace keyword is...<br><br>It can explicitly refer to classes from the current namespace regardless of possibly "use"d classes with the same name from other namespaces. However, this does not apply for functions.<br><br>Example:<br><br>

```
<?php
namespace foo;
class Xyz {}
function abc () {}
?>
```




```
<?php
namespace bar;
class Xyz {}
function abc () {}
?>
```




```
<?php
namespace bar;
use foo\Xyz;
use foo\abc;
new Xyz(); // instantiates \foo\Xyz
new namespace\Xyz(); // instantiates \bar\Xyz
abc(); // invokes \bar\abc regardless of the second use statement
\foo\abc(); // it has to be invoked using the fully qualified name
?>
```
<br><br>Hope, this can save someone from some trouble.<br><br>Best regards.  

---

[Official documentation page](https://www.php.net/manual/en/language.namespaces.nsconstants.php)

**[To root](/README.md)**