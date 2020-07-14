# Callbacks / Callables



You can also use the $this variable to specify a callback:<br><br>

```
<?php
class MyClass {

    public $property = 'Hello World!';

    public function MyMethod()
    {
        call_user_func(array($this, 'myCallbackMethod'));
    }

    public function MyCallbackMethod()
    {
        echo $this->property;
    }

}
?>
```
  

#

Performance note: The callable type hint, like is_callable(), will trigger an autoload of the class if the value looks like a static method callback.  

#

A note on differences when calling callbacks as "variable functions" without the use of call_user_func() (e.g. "

```
<?php $callback = 'printf'; $callback('Hello World!') ?>
```
"):

- Using the name of a function as string has worked since at least 4.3.0
- Calling anonymous functions and invokable objects has worked since 5.3.0
- Using the array structure [$object, 'method'] has worked since 5.4.0

Note, however, that the following are not supported when calling callbacks as variable functions, even though they are supported by call_user_func():

- Calling static class methods via strings such as 'foo::doStuff'
- Calling parent method using the [$object, 'parent::method'] array structure

All of these cases are correctly recognized as callbacks by the 'callable' type hint, however. Thus, the following code will produce an error "Fatal error: Call to undefined function foo::doStuff() in /tmp/code.php on line 4":



```
<?php
class foo {
    static function callIt(callable $callback) {
        $callback();
    }
    
    static function doStuff() {
        echo "Hello World!";
    }
}

foo::callIt('foo::doStuff');
?>
```
<br><br>The code would work fine, if we replaced the &apos;$callback()&apos; with &apos;call_user_func($callback)&apos; or if we used the array [&apos;foo&apos;, &apos;doStuff&apos;] as the callback instead.  

#

You can use &apos;self::methodName&apos; as a callable, but this is dangerous. Consider this example:<br><br>

```
<?php
class Foo {
    public static function doAwesomeThings() {
        FunctionCaller::callIt('self::someAwesomeMethod');
    }

    public static function someAwesomeMethod() {
        // fantastic code goes here.
    }
}

class FunctionCaller {
    public static function callIt(callable $func) {
        call_user_func($func);
    }
}

Foo::doAwesomeThings();
?>
```


This results in an error:
Warning: class 'FunctionCaller' does not have a method 'someAwesomeMethod'.

For this reason you should always use the full class name:


```
<?php
FunctionCaller::callIt('Foo::someAwesomeMethod');
?>
```
<br><br>I believe this is because there is no way for FunctionCaller to know that the string &apos;self&apos; at one point referred to to `Foo`.  

#

When specifying a call back in array notation (ie. array($this, "myfunc") ) the method can be private if called from inside the class, but if you call it from outside you&apos;ll get a warning:<br><br>

```
<?php

class mc {
   public function go(array $arr) {
       array_walk($arr, array($this, "walkIt"));
   }

   private function walkIt($val) {
       echo $val . "&lt;br /&gt;";
   }

    public function export() {
        return array($this, 'walkIt');
    }
}

$data = array(1,2,3,4);

$m = new mc;
$m->go($data); // valid

array_walk($data, $m->export()); // will generate warning

?>
```
<br><br>Output:<br>1&lt;br /&gt;2&lt;br /&gt;3&lt;br /&gt;4&lt;br /&gt;<br>Warning: array_walk() expects parameter 2 to be a valid callback, cannot access private method mc::walkIt() in /in/tfh7f on line 22  

#

you can pass an object as a callable if its class defines the __invoke() magic method..  

#

&gt; As of PHP 5.2.3, it is also possible to pass &apos;ClassName::methodName&apos;<br><br>You can also use &apos;self::methodName&apos;.  This works in PHP 5.2.12 for me.  

#

I needed a function that would determine the type of callable being passed, and, eventually,<br>normalized it to some extent. Here&apos;s what I came up with:<br><br>

```
<?php

/**
 * The callable types and normalizations are given in the table below:
 *
 *  Callable                        | Normalization                   | Type
 * ---------------------------------+---------------------------------+--------------
 *  function (...) use (...) {...}  | function (...) use (...) {...}  | 'closure'
 *  $object                         | $object                         | 'invocable'
 *  "function"                      | "function"                      | 'function'
 *  "class::method"                 | ["class", "method"]             | 'static'
 *  ["class", "parent::method"]     | ["parent of class", "method"]   | 'static'
 *  ["class", "self::method"]       | ["class", "method"]             | 'static'
 *  ["class", "method"]             | ["class", "method"]             | 'static'
 *  [$object, "parent::method"]     | [$object, "parent::method"]     | 'object'
 *  [$object, "self::method"]       | [$object, "method"]             | 'object'
 *  [$object, "method"]             | [$object, "method"]             | 'object'
 * ---------------------------------+---------------------------------+--------------
 *  other callable                  | idem                            | 'unknown'
 * ---------------------------------+---------------------------------+--------------
 *  not a callable                  | null                            | false
 *
 * If the "strict" parameter is set to true, additional checks are
 * performed, in particular:
 *  - when a callable string of the form "class::method" or a callable array
 *    of the form ["class", "method"] is given, the method must be a static one,
 *  - when a callable array of the form [$object, "method"] is given, the
 *    method must be a non-static one.
 *
 */
function callableType($callable, $strict = true, callable&amp; $norm = null) {
  if (!is_callable($callable)) {
    switch (true) {
      case is_object($callable):
        $norm = $callable;
        return 'Closure' === get_class($callable) ? 'closure' : 'invocable';
      case is_string($callable):
        $m    = null;
        if (preg_match('~^(?&lt;class&gt;[a-z_][a-z0-9_]*)::(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i', $callable, $m)) {
          list($left, $right) = [$m['class'], $m['method']];
          if (!$strict || (new \ReflectionMethod($left, $right))->isStatic()) {
            $norm = [$left, $right];
            return 'static';
          }
        } else {
          $norm = $callable;
          return 'function';
        }
        break;
      case is_array($callable):
        $m = null;
        if (preg_match('~^(:?(?&lt;reference&gt;self|parent)::)?(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i', $callable[1], $m)) {
          if (is_string($callable[0])) {
            if ('parent' === strtolower($m['reference'])) {
              list($left, $right) = [get_parent_class($callable[0]), $m['method']];
            } else {
              list($left, $right) = [$callable[0], $m['method']];
            }
            if (!$strict || (new \ReflectionMethod($left, $right))->isStatic()) {
              $norm = [$left, $right];
              return 'static';
            }
          } else {
            if ('self' === strtolower($m['reference'])) {
              list($left, $right) = [$callable[0], $m['method']];
            } else {
              list($left, $right) = $callable;
            }
            if (!$strict || !(new \ReflectionMethod($left, $right))->isStatic()) {
              $norm = [$left, $right];
              return 'object';
            }
          }
        }
        break;
    }
    $norm = $callable;
    return 'unknown';
  }
  $norm = null;
  return false;
}

?>
```
<br><br>Hope someone else finds it useful.  

#

When trying to make a callable from a function name located in a namespace, you MUST give the fully qualified function name (regardless of the current namespace or use statements).<br><br>

```
<?php

namespace MyNamespace;

function doSomethingFancy($arg1)
{
    // do something...
}

$values = [1, 2, 3];

array_map('doSomethingFancy', $values);
// array_map() expects parameter 1 to be a valid callback, function 'doSomethingFancy' not found or invalid function name

array_map('MyNamespace\doSomethingFancy', $values);
// => [..., ..., ...]?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.types.callable.php)

**[To root](/README.md)**