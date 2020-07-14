# Defining namespaces



If your code looks like this:<br><br>

```
<?php
    namespace NS;
?>
```
<br><br>...and you still get "Namespace declaration statement has to be the very first statement in the script" Fatal error, then you probably use UTF-8 encoding (which is good) with Byte Order Mark, aka BOM (which is bad). Try to convert your files to "UTF-8 without BOM", and it should be ok.  

#

Regarding constants defined with define() inside namespaces...<br><br>define() will define constants exactly as specified.  So, if you want to define a constant in a namespace, you will need to specify the namespace in your call to define(), even if you&apos;re calling define() from within a namespace.  The following examples will make it clear.<br><br>The following code will define the constant "MESSAGE" in the global namespace (i.e. "\MESSAGE").<br><br>

```
<?php
namespace test;
define('MESSAGE', 'Hello world!');
?>
```


The following code will define two constants in the "test" namespace.



```
<?php
namespace test;
define('test\HELLO', 'Hello world!');
define(__NAMESPACE__ . '\GOODBYE', 'Goodbye cruel world!');
?>
```
  

#

Expanding on @danbettles note, it is better to always be explicit about which constant to use.<br><br>

```
<?php
    namespace NS;

    define(__NAMESPACE__ .'\foo','111');
    define('foo','222');

    echo foo;  // 111.
    echo \foo;  // 222.
    echo \NS\foo  // 111.
    echo NS\foo  // fatal error. assumes \NS\NS\foo.
?>
```
  

#

"A file containing a namespace must declare the namespace at the top of the file before any other code"<br><br>It might be obvious, but this means that you *can* include comments and white spaces before the namespace keyword.<br><br>

```
<?php
// Lots 
// of
// interesting
// comments and white space

namespace Foo;
class Bar {
}
?>
```
  

#

You should not try to create namespaces that use PHP keywords. These will cause parse errors. <br><br>Examples:<br><br>

```
<?php
namespace Project/Classes/Function; // Causes parse errors
namespace Project/Abstract/Factory; // Causes parse errors
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definition.php)

**[To root](/README.md)**