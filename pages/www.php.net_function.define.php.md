# define



Be aware that if "Notice"-level error reporting is turned off, then trying to use a constant as a variable will result in it being interpreted as a string, if it has not been defined.<br><br>I was working on a program which included a config file which contained:<br><br>

```
<?php
define('ENABLE_UPLOADS', true);
?>
```


Since I wanted to remove the ability for uploads, I changed the file to read:



```
<?php
//define('ENABLE_UPLOADS', true);
?>
```


However, to my surprise, the program was still allowing uploads. Digging deeper into the code, I discovered this:



```
<?php
if ( ENABLE_UPLOADS ):
?>
```
<br><br>Since &apos;ENABLE_UPLOADS&apos; was not defined as a constant, PHP was interpreting its use as a string constant, which of course evaluates as True.  

---

define() will define constants exactly as specified.  So, if you want to define a constant in a namespace, you will need to specify the namespace in your call to define(), even if you&apos;re calling define() from within a namespace.  The following examples will make it clear.<br><br>The following code will define the constant "MESSAGE" in the global namespace (i.e. "\MESSAGE").<br><br>

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
  

---

[Official documentation page](https://www.php.net/manual/en/function.define.php)

**[To root](/README.md)**