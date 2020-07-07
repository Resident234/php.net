# Callbacks / Callables





You can also use the $this variable to specify a callback:





```
<?php

class MyClass {



&#xA0; &#xA0; public $property = &apos;Hello World!&apos;;



&#xA0; &#xA0; public function MyMethod()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; call_user_func(array($this, &apos;myCallbackMethod&apos;));

&#xA0; &#xA0; }



&#xA0; &#xA0; public function MyCallbackMethod()

&#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; echo $this-&gt;property;

&#xA0; &#xA0; }



}

?>
```



  

#



Performance note: The callable type hint, like is_callable(), will trigger an autoload of the class if the value looks like a static method callback.

  

#



A note on differences when calling callbacks as &quot;variable functions&quot; without the use of call_user_func() (e.g. &quot;

```
<?php $callback = &apos;printf&apos;; $callback(&apos;Hello World!&apos;) ?>
```
&quot;):

- Using the name of a function as string has worked since at least 4.3.0
- Calling anonymous functions and invokable objects has worked since 5.3.0
- Using the array structure [$object, &apos;method&apos;] has worked since 5.4.0

Note, however, that the following are not supported when calling callbacks as variable functions, even though they are supported by call_user_func():

- Calling static class methods via strings such as &apos;foo::doStuff&apos;
- Calling parent method using the [$object, &apos;parent::method&apos;] array structure

All of these cases are correctly recognized as callbacks by the &apos;callable&apos; type hint, however. Thus, the following code will produce an error &quot;Fatal error: Call to undefined function foo::doStuff() in /tmp/code.php on line 4&quot;:



```
<?php
class foo {
&#xA0; &#xA0; static function callIt(callable $callback) {
&#xA0; &#xA0; &#xA0; &#xA0; $callback();
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; static function doStuff() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Hello World!&quot;;
&#xA0; &#xA0; }
}

foo::callIt(&apos;foo::doStuff&apos;);
?>
```


The code would work fine, if we replaced the &apos;$callback()&apos; with &apos;call_user_func($callback)&apos; or if we used the array [&apos;foo&apos;, &apos;doStuff&apos;] as the callback instead.

  

#



You can use &apos;self::methodName&apos; as a callable, but this is dangerous. Consider this example:



```
<?php
class Foo {
&#xA0; &#xA0; public static function doAwesomeThings() {
&#xA0; &#xA0; &#xA0; &#xA0; FunctionCaller::callIt(&apos;self::someAwesomeMethod&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; public static function someAwesomeMethod() {
&#xA0; &#xA0; &#xA0; &#xA0; // fantastic code goes here.
&#xA0; &#xA0; }
}

class FunctionCaller {
&#xA0; &#xA0; public static function callIt(callable $func) {
&#xA0; &#xA0; &#xA0; &#xA0; call_user_func($func);
&#xA0; &#xA0; }
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


I believe this is because there is no way for FunctionCaller to know that the string &apos;self&apos; at one point referred to to `Foo`.

  

#



When specifying a call back in array notation (ie. array($this, &quot;myfunc&quot;) ) the method can be private if called from inside the class, but if you call it from outside you&apos;ll get a warning:





```
<?php



class mc {

&#xA0;&#xA0; public function go(array $arr) {

&#xA0; &#xA0; &#xA0;&#xA0; array_walk($arr, array($this, &quot;walkIt&quot;));

&#xA0;&#xA0; }



&#xA0;&#xA0; private function walkIt($val) {

&#xA0; &#xA0; &#xA0;&#xA0; echo $val . &quot;&lt;br /&gt;&quot;;

&#xA0;&#xA0; }



&#xA0; &#xA0; public function export() {

&#xA0; &#xA0; &#xA0; &#xA0; return array($this, &apos;walkIt&apos;);

&#xA0; &#xA0; }

}



$data = array(1,2,3,4);



$m = new mc;

$m-&gt;go($data); // valid



array_walk($data, $m-&gt;export()); // will generate warning



?>
```




Output:

1&lt;br /&gt;2&lt;br /&gt;3&lt;br /&gt;4&lt;br /&gt;

Warning: array_walk() expects parameter 2 to be a valid callback, cannot access private method mc::walkIt() in /in/tfh7f on line 22

  

#



you can pass an object as a callable if its class defines the __invoke() magic method..

  

#



&gt; As of PHP 5.2.3, it is also possible to pass &apos;ClassName::methodName&apos;

You can also use &apos;self::methodName&apos;.&#xA0; This works in PHP 5.2.12 for me.

  

#



I needed a function that would determine the type of callable being passed, and, eventually,
normalized it to some extent. Here&apos;s what I came up with:



```
<?php

/**
 * The callable types and normalizations are given in the table below:
 *
 *&#xA0; Callable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | Normalization&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | Type
 * ---------------------------------+---------------------------------+--------------
 *&#xA0; function (...) use (...) {...}&#xA0; | function (...) use (...) {...}&#xA0; | &apos;closure&apos;
 *&#xA0; $object&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | $object&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;invocable&apos;
 *&#xA0; &quot;function&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | &quot;function&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | &apos;function&apos;
 *&#xA0; &quot;class::method&quot;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;static&apos;
 *&#xA0; [&quot;class&quot;, &quot;parent::method&quot;]&#xA0; &#xA0;&#xA0; | [&quot;parent of class&quot;, &quot;method&quot;]&#xA0;&#xA0; | &apos;static&apos;
 *&#xA0; [&quot;class&quot;, &quot;self::method&quot;]&#xA0; &#xA0; &#xA0;&#xA0; | [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;static&apos;
 *&#xA0; [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | [&quot;class&quot;, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;static&apos;
 *&#xA0; [$object, &quot;parent::method&quot;]&#xA0; &#xA0;&#xA0; | [$object, &quot;parent::method&quot;]&#xA0; &#xA0;&#xA0; | &apos;object&apos;
 *&#xA0; [$object, &quot;self::method&quot;]&#xA0; &#xA0; &#xA0;&#xA0; | [$object, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;object&apos;
 *&#xA0; [$object, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | [$object, &quot;method&quot;]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; | &apos;object&apos;
 * ---------------------------------+---------------------------------+--------------
 *&#xA0; other callable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | idem&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | &apos;unknown&apos;
 * ---------------------------------+---------------------------------+--------------
 *&#xA0; not a callable&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | null&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; | false
 *
 * If the &quot;strict&quot; parameter is set to true, additional checks are
 * performed, in particular:
 *&#xA0; - when a callable string of the form &quot;class::method&quot; or a callable array
 *&#xA0; &#xA0; of the form [&quot;class&quot;, &quot;method&quot;] is given, the method must be a static one,
 *&#xA0; - when a callable array of the form [$object, &quot;method&quot;] is given, the
 *&#xA0; &#xA0; method must be a non-static one.
 *
 */
function callableType($callable, $strict = true, callable&amp; $norm = null) {
&#xA0; if (!is_callable($callable)) {
&#xA0; &#xA0; switch (true) {
&#xA0; &#xA0; &#xA0; case is_object($callable):
&#xA0; &#xA0; &#xA0; &#xA0; $norm = $callable;
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;Closure&apos; === get_class($callable) ? &apos;closure&apos; : &apos;invocable&apos;;
&#xA0; &#xA0; &#xA0; case is_string($callable):
&#xA0; &#xA0; &#xA0; &#xA0; $m&#xA0; &#xA0; = null;
&#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(&apos;~^(?&lt;class&gt;[a-z_][a-z0-9_]*)::(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i&apos;, $callable, $m)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($left, $right) = [$m[&apos;class&apos;], $m[&apos;method&apos;]];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$strict || (new \ReflectionMethod($left, $right))-&gt;isStatic()) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $norm = [$left, $right];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;static&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $norm = $callable;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;function&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; case is_array($callable):
&#xA0; &#xA0; &#xA0; &#xA0; $m = null;
&#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(&apos;~^(:?(?&lt;reference&gt;self|parent)::)?(?&lt;method&gt;[a-z_][a-z0-9_]*)$~i&apos;, $callable[1], $m)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (is_string($callable[0])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;parent&apos; === strtolower($m[&apos;reference&apos;])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($left, $right) = [get_parent_class($callable[0]), $m[&apos;method&apos;]];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($left, $right) = [$callable[0], $m[&apos;method&apos;]];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$strict || (new \ReflectionMethod($left, $right))-&gt;isStatic()) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $norm = [$left, $right];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;static&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (&apos;self&apos; === strtolower($m[&apos;reference&apos;])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($left, $right) = [$callable[0], $m[&apos;method&apos;]];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; list($left, $right) = $callable;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$strict || !(new \ReflectionMethod($left, $right))-&gt;isStatic()) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $norm = [$left, $right];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;object&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }
&#xA0; &#xA0; $norm = $callable;
&#xA0; &#xA0; return &apos;unknown&apos;;
&#xA0; }
&#xA0; $norm = null;
&#xA0; return false;
}

?>
```


Hope someone else finds it useful.

  

#



When trying to make a callable from a function name located in a namespace, you MUST give the fully qualified function name (regardless of the current namespace or use statements).



```
<?php

namespace MyNamespace;

function doSomethingFancy($arg1)
{
&#xA0; &#xA0; // do something...
}

$values = [1, 2, 3];

array_map(&apos;doSomethingFancy&apos;, $values);
// array_map() expects parameter 1 to be a valid callback, function &apos;doSomethingFancy&apos; not found or invalid function name

array_map(&apos;MyNamespace\doSomethingFancy&apos;, $values);
// =&gt; [..., ..., ...]


  

#

[Official documentation page](https://www.php.net/manual/en/language.types.callable.php)

**[To root](/README.md)**