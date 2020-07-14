# Callbacks / Callables



You can also use the $this variable to specify a callback:<br><br>

```
<?php
class MyClass {

    public $property = &apos;Hello World!&apos;;

    public function MyMethod()
    {
        call_user_func(array($this, &apos;myCallbackMethod&apos;));
    }

    public function MyCallbackMethod()
    {
        echo $this-&gt;property;
    }

}
?>
```
  

#

Performance note: The callable type hint, like is_callable(), will trigger an autoload of the class if the value looks like a static method callback.  

#

A note on differences when calling callbacks as "variable functions" without the use of call_user_func() (e.g. "

```
<?php $callback = &apos;printf&apos;; $callback(&apos;Hello World!&apos;) ?>
```
"):

- Using the name of a function as string has worked since at least 4.3.0
- Calling anonymous functions and invokable objects has worked since 5.3.0
- Using the array structure [$object, &apos;method&apos;] has worked since 5.4.0

Note, however, that the following are not supported when calling callbacks as variable functions, even though they are supported by call_user_func():

- Calling static class methods via strings such as &apos;foo::doStuff&apos;
- Calling parent method using the [$object, &apos;parent::method&apos;] array structure

All of these cases are correctly recognized as callbacks by the &apos;callable&apos; type hint, however. Thus, the following code will produce an error "Fatal error: Call to undefined function foo::doStuff() in /tmp/code.php on line 4":



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

foo::callIt(&apos;foo::doStuff&apos;);
?>
```
<br><br>The code would work fine, if we replaced the &apos;$callback()&apos; with &apos;call_user_func($callback)&apos; or if we used the array [&apos;foo&apos;, &apos;doStuff&apos;] as the callback instead.  

#

You can use &apos;self::methodName&apos; as a callable, but this is dangerous. Consider this example:<br><br>

```
<?php
class Foo {
    public static function doAwesomeThings() {
        FunctionCaller::callIt(&apos;self::someAwesomeMethod&apos;);
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
Warning: class &apos;FunctionCaller&apos; does not have a method &apos;someAwesomeMethod&apos;.

For this reason you should always use the full class name:


```
<?php
FunctionCaller::callIt(&apos;Foo::someAwesomeMethod&apos;);
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
        return array($this, &apos;walkIt&apos;);
    }
}

$data = array(1,2,3,4);

$m = new mc;
$m-&gt;go($data); // valid

array_walk($data, $m-&gt;export()); // will generate warning

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
 *  function (...) use (...) {...}  | function (...) use (...) {...}  | &apos;closure&apos;
 *  $object                         | $object                         | &apos;invocable&apos;
 *  "function"                      | "function"                      | &apos;function&apos;
 *  "class::method"                 | ["class", "method"]             | &apos;static&apos;
 *  ["class", "parent::method"]     | ["parent of class", "method"]   | &apos;static&apos;
 *  ["class", "self::method"]       | ["class", "method"]             | &apos;static&apos;
 *  ["class", "method"]             | ["class", "method"]             | &apos;static&apos;
 *  [$object, "parent::method"]     | [$object, "parent::method"]     | &apos;object&apos;
 *  [$object, "self::method"]       | [$object, "method"]             | &apos;object&apos;
 *  [$object, "method"]             | [$object, "method"]             | &apos;object&apos;
 * ---------------------------------+---------------------------------+--------------
 *  other callable                  | idem                            | &apos;unknown&apos;
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
        return &apos;Closure&apos; === get_class($callable) ? &apos;closure&apos; : &apos;invocable&apos;;
      case is_string($callable):
        $m    = null;
        if (preg_match(&apos;~^(?&lt;class&gt;[a-z_][a-z0-9_]*)::(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i&apos;, $callable, $m)) {
          list($left, $right) = [$m[&apos;class&apos;], $m[&apos;method&apos;]];
          if (!$strict || (new \ReflectionMethod($left, $right))-&gt;isStatic()) {
            $norm = [$left, $right];
            return &apos;static&apos;;
          }
        } else {
          $norm = $callable;
          return &apos;function&apos;;
        }
        break;
      case is_array($callable):
        $m = null;
        if (preg_match(&apos;~^(:?(?&lt;reference&gt;self|parent)::)?(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i&apos;, $callable[1], $m)) {
          if (is_string($callable[0])) {
            if (&apos;parent&apos; === strtolower($m[&apos;reference&apos;])) {
              list($left, $right) = [get_parent_class($callable[0]), $m[&apos;method&apos;]];
            } else {
              list($left, $right) = [$callable[0], $m[&apos;method&apos;]];
            }
            if (!$strict || (new \ReflectionMethod($left, $right))-&gt;isStatic()) {
              $norm = [$left, $right];
              return &apos;static&apos;;
            }
          } else {
            if (&apos;self&apos; === strtolower($m[&apos;reference&apos;])) {
              list($left, $right) = [$callable[0], $m[&apos;method&apos;]];
            } else {
              list($left, $right) = $callable;
            }
            if (!$strict || !(new \ReflectionMethod($left, $right))-&gt;isStatic()) {
              $norm = [$left, $right];
              return &apos;object&apos;;
            }
          }
        }
        break;
    }
    $norm = $callable;
    return &apos;unknown&apos;;
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
<?php<br><br>namespace MyNamespace;<br><br>function doSomethingFancy($arg1)<br>{<br>    // do something...<br>}<br><br>$values = [1, 2, 3];<br><br>array_map(&apos;doSomethingFancy&apos;, $values);<br>// array_map() expects parameter 1 to be a valid callback, function &apos;doSomethingFancy&apos; not found or invalid function name<br><br>array_map(&apos;MyNamespace\doSomethingFancy&apos;, $values);<br>// =&gt; [..., ..., ...]  

#

[Official documentation page](https://www.php.net/manual/en/language.types.callable.php)

**[To root](/README.md)**