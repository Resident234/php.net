# Anonymous functions





Watch out when &apos;importing&apos; variables to a closure&apos;s scope&#xA0; -- it&apos;s easy to miss / forget that they are actually being *copied* into the closure&apos;s scope, rather than just being made available.

So you will need to explicitly pass them in by reference if your closure cares about their contents over time:



```
<?php
$result = 0;

$one = function()
{ var_dump($result); };

$two = function() use ($result)
{ var_dump($result); };

$three = function() use (&amp;$result)
{ var_dump($result); };

$result++;

$one();&#xA0; &#xA0; // outputs NULL: $result is not in scope
$two();&#xA0; &#xA0; // outputs int(0): $result was copied
$three();&#xA0; &#xA0; // outputs int(1)
?>
```


Another less trivial example with objects (what I actually tripped up on):



```
<?php
//set up variable in advance
$myInstance = null;

$broken = function() uses ($myInstance)
{
&#xA0; &#xA0; if(!empty($myInstance)) $myInstance-&gt;doSomething();
};

$working = function() uses (&amp;$myInstance)
{
&#xA0; &#xA0; if(!empty($myInstance)) $myInstance-&gt;doSomething();
}

//$myInstance might be instantiated, might not be
if(SomeBusinessLogic::worked() == true)
{
&#xA0; &#xA0; $myInstance = new myClass();
}

$broken();&#xA0; &#xA0; // will never do anything: $myInstance will ALWAYS be null inside this closure.
$working();&#xA0; &#xA0; // will call doSomething if $myInstance is instantiated

?>
```



  

#



To recursively call a closure, use this code.





```
<?php

$recursive = function () use (&amp;$recursive){

&#xA0; &#xA0; // The function is now available as $recursive

}

?>
```




This DOES NOT WORK





```
<?php

$recursive = function () use ($recursive){

&#xA0; &#xA0; // The function is now available as $recursive

}

?>
```



  

#



You may have been disapointed if you tried to call a closure stored in an instance variable as you would regularly do with methods:



```
<?php

$obj = new StdClass();

$obj-&gt;func = function(){
 echo &quot;hello&quot;;
};

//$obj-&gt;func(); // doesn&apos;t work! php tries to match an instance method called &quot;func&quot; that is not defined in the original class&apos; signature

// you have to do this instead:
$func = $obj-&gt;func;
$func();

// or:
call_user_func($obj-&gt;func);

// however, you might wanna check this out:
$array[&apos;func&apos;] = function(){
 echo &quot;hello&quot;;
};

$array[&apos;func&apos;](); // it works! i discovered that just recently ;)
?>
```


Now, coming back to the problem of assigning functions/methods &quot;on the fly&quot; to an object and being able to call them as if they were regular methods, you could trick php with this lawbreaker-code:



```
<?php
class test{
 private $functions = array();
 private $vars = array();
 
 function __set($name,$data)
 {
&#xA0; if(is_callable($data))
&#xA0; &#xA0; $this-&gt;functions[$name] = $data;
&#xA0; else
&#xA0;&#xA0; $this-&gt;vars[$name] = $data;
 }
 
 function __get($name)
 {
&#xA0; if(isset($this-&gt;vars[$name]))
&#xA0;&#xA0; return $this-&gt;vars[$name];
 }
 
 function __call($method,$args)
 {
&#xA0; if(isset($this-&gt;functions[$method]))
&#xA0; {
&#xA0;&#xA0; call_user_func_array($this-&gt;functions[$method],$args);
&#xA0; } else {
&#xA0;&#xA0; // error out
&#xA0; }
 }
}

// LET&apos;S BREAK SOME LAW NOW!
$obj = new test;

$obj-&gt;sayHelloWithMyName = function($name){
 echo &quot;Hello $name!&quot;;
};

$obj-&gt;sayHelloWithMyName(&apos;Fabio&apos;); // Hello Fabio!

// THE OLD WAY (NON-CLOSURE) ALSO WORKS:

function sayHello()
{
 echo &quot;Hello!&quot;;
}

$obj-&gt;justSayHello = &apos;sayHello&apos;;
$obj-&gt;justSayHello(); // Hello!
?>
```


NOTICE: of course this is very bad practice since you cannot refere to protected or private fields/methods inside these pseudo &quot;methods&quot; as they are not instance methods at all but rather ordinary functions/closures assigned to the object&apos;s instance variables &quot;on the fly&quot;. But I hope you&apos;ve enjoyed the jurney ;)

  

#



In case you were wondering (cause i was), anonymous functions can return references just like named functions can.&#xA0; Simply use the &amp; the same way you would for a named function...right after the `function` keyword (and right before the nonexistent name).



```
<?php
&#xA0; &#xA0; $value = 0;
&#xA0; &#xA0; $fn = function &amp;() use (&amp;$value) { return $value; };

&#xA0; &#xA0; $x =&amp; $fn();
&#xA0; &#xA0; var_dump($x, $value);&#xA0; &#xA0; &#xA0; &#xA0; // &apos;int(0)&apos;, &apos;int(0)&apos;
&#xA0; &#xA0; ++$x;
&#xA0; &#xA0; var_dump($x, $value);&#xA0; &#xA0; &#xA0; &#xA0; // &apos;int(1)&apos;, &apos;int(1)&apos;


  

#





```
<?php
&#xA0; &#xA0; /*
&#xA0; &#xA0; (string) $name Name of the function that you will add to class.
&#xA0; &#xA0; Usage : $Foo-&gt;add(function(){},$name);
&#xA0; &#xA0; This will add a public function in Foo Class.
&#xA0; &#xA0; */
&#xA0; &#xA0; class Foo
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; public function add($func,$name)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;{$name} = $func;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; public function __call($func,$arguments){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; call_user_func_array($this-&gt;{$func}, $arguments); 
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; $Foo = new Foo();
&#xA0; &#xA0; $Foo-&gt;add(function(){
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Hello World&quot;;
&#xA0; &#xA0; },&quot;helloWorldFunction&quot;);
&#xA0; &#xA0; $Foo-&gt;add(function($parameterone){
&#xA0; &#xA0; &#xA0; &#xA0; echo $parameterone;
&#xA0; &#xA0; },&quot;exampleFunction&quot;);
&#xA0; &#xA0; $Foo-&gt;helloWorldFunction(); /*Output : Hello World*/
&#xA0; &#xA0; $Foo-&gt;exampleFunction(&quot;Hello PHP&quot;); /*Output : Hello PHP*/
?>
```



  

#



If you want to create and then immediately call a closure directly, in-line, and immediately get its return value (instead of the closure reference itself), then the proper syntax is as follows:



```
<?php

$a = &apos;foo&apos;; $b = &apos;bar&apos;;
$test = (function() use($a,$b) { return $a . $b; })();
echo $test;

?>
```


As for why you would want to do that? Well, that&apos;s up to you. I&apos;m sure there are some legitimate reasons. It&apos;s a pretty common pattern in some other famous scripting languages. But if you&apos;re doing this in PHP, you should think carefully and ask yourself if you really have a good reason for it, or if you should just go and re-structure your code instead. ;-)

  

#



If you want to make sure that one of the parameters of your function is a Closure, you can use Type Hinting.
see: http://php.net/manual/en/language.oop5.typehinting.php

Example:


```
<?php

class TheRoot
{
&#xA0; &#xA0; public function poidh($param) {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;TheRoot $param!&quot;;
&#xA0; &#xA0; }&#xA0;&#xA0; 

}

class Internet
{
&#xA0; &#xA0; # here, $my_closure must be of type object Closure
&#xA0; &#xA0; public function run_my_closure($bar, Closure $my_closure) {
&#xA0; &#xA0; &#xA0; &#xA0; $my_closure($bar);
&#xA0; &#xA0; }&#xA0;&#xA0; 
}

$Internet = new Internet();
$Root = new TheRoot();

$Internet-&gt;run_my_closure($Root, function($Object) {
&#xA0; &#xA0; $Object-&gt;poidh(42);
});

?>
```

The above code simply yields:
&quot;TheRoot 42!&quot;

NOTE: If you are using namespaces, make sure you give a fully qualified namespace.

print_r() of Internet::run_my_closure&apos;s $my_closure


```
<?php
Closure Object
(
&#xA0; &#xA0; [parameter] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [$Object] =&gt; 
&#xA0; &#xA0; &#xA0; &#xA0; )

)
?>
```


var_dump() of Internet::run_my_closure&apos;s $my_closure


```
<?php
object(Closure)#3 (1) {
&#xA0; [&quot;parameter&quot;]=&gt;
&#xA0; array(1) {
&#xA0; &#xA0; [&quot;$Object&quot;]=&gt;
&#xA0; &#xA0; string(10) &quot;&quot;
&#xA0; }
}
?>
```



  

#



When using anonymous functions as properties in Classes, note that there are three name scopes: one for constants, one for properties and one for methods. That means, you can use the same name for a constant, for a property and for a method at a time.

Since a property can be also an anonymous function as of PHP 5.3.0, an oddity arises when they share the same name, not meaning that there would be any conflict.

Consider the following example:



```
<?php
&#xA0; &#xA0; class MyClass {
&#xA0; &#xA0; &#xA0; &#xA0; const member = 1;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public $member;
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public function member () {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &quot;method &apos;member&apos;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; public function __construct () {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;member = function () {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &quot;anonymous function &apos;member&apos;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; };
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; header(&quot;Content-Type: text/plain&quot;);
&#xA0; &#xA0; 
&#xA0; &#xA0; $myObj = new MyClass();

&#xA0; &#xA0; var_dump(MyClass::member);&#xA0; // int(1)
&#xA0; &#xA0; var_dump($myObj-&gt;member);&#xA0;&#xA0; // object(Closure)#2 (0) {}
&#xA0; &#xA0; var_dump($myObj-&gt;member()); // string(15) &quot;method &apos;member&apos;&quot;
&#xA0; &#xA0; $myMember = $myObj-&gt;member;
&#xA0; &#xA0; var_dump($myMember());&#xA0; &#xA0; &#xA0; // string(27) &quot;anonymous function &apos;member&apos;&quot;
?>
```


That means, regular method invocations work like expected and like before. The anonymous function instead, must be retrieved into a variable first (just like a property) and can only then be invoked.

Best regards,

  

#



PERFORMANCE BENCHMARK 2017!

I decided to compare a single, saved closure against constantly creating the same anonymous closure on every loop iteration. And I tried 10 million loop iterations, in PHP 7.0.14 from Dec 2016. Result:

a single saved closure kept in a variable and re-used (10000000 iterations): 1.3874590396881 seconds

new anonymous closure created each time (10000000 iterations): 2.8460240364075 seconds

In other words, over the course of 10 million iterations, creating the closure again during every iteration only added a total of &quot;1.459 seconds&quot; to the runtime. So that means that every creation of a new anonymous closure takes about 146 nanoseconds on my 7 years old dual-core laptop. I guess PHP keeps a cached &quot;template&quot; for the anonymous function and therefore doesn&apos;t need much time to create a new instance of the closure!

So you do NOT have to worry about constantly re-creating your anonymous closures over and over again in tight loops! At least not as of PHP 7! There is absolutely NO need to save an instance in a variable and re-use it. And not being restricted by that is a great thing, because it means you can feel free to use anonymous functions exactly where they matter, as opposed to defining them somewhere else in the code. :-)

  

#



As an alternative to gabriel&apos;s recursive construction, you may instead assign the recursive function to a variable, and use it by reference, thus:



```
<?php
$fib = function($n)use(&amp;$fib)
{
&#xA0; &#xA0; if($n == 0 || $n == 1) return 1;
&#xA0; &#xA0; return $fib($n - 1) + $fib($n - 2);
};

echo $fib(10);
?>
```

Hardly a sensible implementation of the Fibonacci sequence, but that&apos;s not the point! The point is that the variable needs to be used by reference, not value.

Without the &apos;&amp;&apos;, the anonymous function gets the value of $fib at the time the function is being created. But until the function has been created, $fib can&apos;t have it as a value! It&apos;s not until AFTER the function has been assigned to $fib that $fib can be used to call the function - but by then it&apos;s too late to pass its value to the function being created!

Using a reference resolves the dilemma: when called, the anonymous function will use $fib&apos;s current value, which will be the anonymous function itself.

At least, it will be if you don&apos;t reassign $fib to anything else between creating the function and calling it:



```
<?php
$fib = function($n)use(&amp;$fib)
{
&#xA0; &#xA0; if($n == 0 || $n == 1) return 1;
&#xA0; &#xA0; return $fib($n - 1) + $fib($n - 2);
};

$lie = $fib;

$fib = function($n)
{
&#xA0; &#xA0; return 100;
};

echo $lie(10); // 200, because $fib(10 - 1) and $fib(10 - 2) both return 100.
?>
```


Of course, that&apos;s true of any variable: if you don&apos;t want its value to change, don&apos;t change its value.

All the usual scoping rules for variables still apply: a local variable in a function is a different variable from another one with the same name in another function:



```
<?php
$fib = function($n)use(&amp;$fib)
{
&#xA0; &#xA0; if($n == 0 || $n == 1) return 1;
&#xA0; &#xA0; return $fib($n - 1) + $fib($n - 2);
};

$bark = function($f)
{
&#xA0; &#xA0; $fib = &apos;cake&apos;;&#xA0; &#xA0; // A totally different variable from the $fib above.
&#xA0; &#xA0; return 2 * $f(5);
};

echo $bark($fib); // 16, twice the fifth Fibonacci number

?>
```



  

#



Anonymous functions are great for events!



```
<?php

class Event {

&#xA0; public static $events = array();
&#xA0; 
&#xA0; public static function bind($event, $callback, $obj = null) {
&#xA0; &#xA0; if (!self::$events[$event]) {
&#xA0; &#xA0; &#xA0; self::$events[$event] = array();
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; self::$events[$event][] = ($obj === null)&#xA0; ? $callback : array($obj, $callback);
&#xA0; }
&#xA0; 
&#xA0; public static function run($event) {
&#xA0; &#xA0; if (!self::$events[$event]) return;
&#xA0; &#xA0; 
&#xA0; &#xA0; foreach (self::$events[$event] as $callback) {
&#xA0; &#xA0; &#xA0; if (call_user_func($callback) === false) break;
&#xA0; &#xA0; }
&#xA0; }

}

function hello() {
&#xA0; echo &quot;Hello from function hello()\n&quot;;
}

class Foo {
&#xA0; function hello() {
&#xA0; &#xA0; echo &quot;Hello from foo-&gt;hello()\n&quot;;
&#xA0; }
}

class Bar {
&#xA0; function hello() {
&#xA0; &#xA0; echo &quot;Hello from Bar::hello()\n&quot;;
&#xA0; }
}

$foo = new Foo();

// bind a global function to the &apos;test&apos; event
Event::bind(&quot;test&quot;, &quot;hello&quot;);

// bind an anonymous function
Event::bind(&quot;test&quot;, function() { echo &quot;Hello from anonymous function\n&quot;; });

// bind an class function on an instance
Event::bind(&quot;test&quot;, &quot;hello&quot;, $foo);

// bind a static class function
Event::bind(&quot;test&quot;, &quot;Bar::hello&quot;);

Event::run(&quot;test&quot;);

/* Output
Hello from function hello()
Hello from anonymous function
Hello from foo-&gt;hello()
Hello from Bar::hello()
*/

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/functions.anonymous.php)

**[To root](/README.md)**