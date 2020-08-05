# function_exists



You can use this function to conditionally define functions, see: http://php.net/manual/en/functions.user-defined.php<br><br>For instance Wordpress uses it to make functions "pluggable." If a plugin has already defined a pluggable function, then the WP code knows not to try to redefine it.<br><br>But function_exists() will always return true unless you wrap any later function definition in a conditional clause, like if(){...}. This is a subtle matter in PHP parsing. Some examples:<br><br>

```
<?php
if (function_exists('foo')) {
  print "foo defined\\n";
} else {
  print "foo not defined\\n";
}
function foo() {}

if (function_exists('bar')) {
  print "bar defined\\n";
} else {
  print "defining bar\\n";
  function bar() {}
}
print "calling bar\\n";
bar(); // ok to call function conditionally defined earlier

print "calling baz\\n";
baz(); // ok to call function unconditionally defined later
function baz() {}

qux(); // NOT ok to call function conditionally defined later
if (!function_exists('qux')) {
  function qux() {}
}
?>
```
<br>Prints:<br>  foo defined<br>  defining bar<br>  calling bar<br>  calling baz<br>  PHP Fatal error: Call to undefined function qux()<br><br>Any oddities are probably due to the order in which you include/require files.  

---

It should be noted that the function_exists check is not relative to the root namespace. This means that the namespace should be appended to the check:<br><br>

```
<?php

  namespace test;

  if (!function_exists(__NAMESPACE__ . '\example'))
  {

    function example()
    {

    }

  }

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.function-exists.php)

**[To root](/README.md)**