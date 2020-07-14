# Anonymous functions



Watch out when &apos;importing&apos; variables to a closure&apos;s scope  -- it&apos;s easy to miss / forget that they are actually being *copied* into the closure&apos;s scope, rather than just being made available.<br><br>So you will need to explicitly pass them in by reference if your closure cares about their contents over time:<br><br>

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

$one();    // outputs NULL: $result is not in scope
$two();    // outputs int(0): $result was copied
$three();    // outputs int(1)
?>
```


Another less trivial example with objects (what I actually tripped up on):



```
<?php
//set up variable in advance
$myInstance = null;

$broken = function() uses ($myInstance)
{
    if(!empty($myInstance)) $myInstance-&gt;doSomething();
};

$working = function() uses (&amp;$myInstance)
{
    if(!empty($myInstance)) $myInstance-&gt;doSomething();
}

//$myInstance might be instantiated, might not be
if(SomeBusinessLogic::worked() == true)
{
    $myInstance = new myClass();
}

$broken();    // will never do anything: $myInstance will ALWAYS be null inside this closure.
$working();    // will call doSomething if $myInstance is instantiated

?>
```
  

#

To recursively call a closure, use this code.<br><br>

```
<?php
$recursive = function () use (&amp;$recursive){
    // The function is now available as $recursive
}
?>
```


This DOES NOT WORK



```
<?php
$recursive = function () use ($recursive){
    // The function is now available as $recursive
}
?>
```
  

#

You may have been disapointed if you tried to call a closure stored in an instance variable as you would regularly do with methods:<br><br>

```
<?php

$obj = new StdClass();

$obj-&gt;func = function(){
 echo "hello";
};

//$obj-&gt;func(); // doesn&apos;t work! php tries to match an instance method called "func" that is not defined in the original class&apos; signature

// you have to do this instead:
$func = $obj-&gt;func;
$func();

// or:
call_user_func($obj-&gt;func);

// however, you might wanna check this out:
$array[&apos;func&apos;] = function(){
 echo "hello";
};

$array[&apos;func&apos;](); // it works! i discovered that just recently ;)
?>
```


Now, coming back to the problem of assigning functions/methods "on the fly" to an object and being able to call them as if they were regular methods, you could trick php with this lawbreaker-code:



```
<?php
class test{
 private $functions = array();
 private $vars = array();
 
 function __set($name,$data)
 {
  if(is_callable($data))
    $this-&gt;functions[$name] = $data;
  else
   $this-&gt;vars[$name] = $data;
 }
 
 function __get($name)
 {
  if(isset($this-&gt;vars[$name]))
   return $this-&gt;vars[$name];
 }
 
 function __call($method,$args)
 {
  if(isset($this-&gt;functions[$method]))
  {
   call_user_func_array($this-&gt;functions[$method],$args);
  } else {
   // error out
  }
 }
}

// LET&apos;S BREAK SOME LAW NOW!
$obj = new test;

$obj-&gt;sayHelloWithMyName = function($name){
 echo "Hello $name!";
};

$obj-&gt;sayHelloWithMyName(&apos;Fabio&apos;); // Hello Fabio!

// THE OLD WAY (NON-CLOSURE) ALSO WORKS:

function sayHello()
{
 echo "Hello!";
}

$obj-&gt;justSayHello = &apos;sayHello&apos;;
$obj-&gt;justSayHello(); // Hello!
?>
```
<br><br>NOTICE: of course this is very bad practice since you cannot refere to protected or private fields/methods inside these pseudo "methods" as they are not instance methods at all but rather ordinary functions/closures assigned to the object&apos;s instance variables "on the fly". But I hope you&apos;ve enjoyed the jurney ;)  

#

In case you were wondering (cause i was), anonymous functions can return references just like named functions can.  Simply use the &amp; the same way you would for a named function...right after the `function` keyword (and right before the nonexistent name).<br><br>

```
<?php<br>    $value = 0;<br>    $fn = function &amp;() use (&amp;$value) { return $value; };<br><br>    $x =&amp; $fn();<br>    var_dump($x, $value);        // &apos;int(0)&apos;, &apos;int(0)&apos;<br>    ++$x;<br>    var_dump($x, $value);        // &apos;int(1)&apos;, &apos;int(1)&apos;  

#



```
<?php
    /*
    (string) $name Name of the function that you will add to class.
    Usage : $Foo-&gt;add(function(){},$name);
    This will add a public function in Foo Class.
    */
    class Foo
    {
        public function add($func,$name)
        {
            $this-&gt;{$name} = $func;
        }
        public function __call($func,$arguments){
            call_user_func_array($this-&gt;{$func}, $arguments); 
        }
    }
    $Foo = new Foo();
    $Foo-&gt;add(function(){
        echo "Hello World";
    },"helloWorldFunction");
    $Foo-&gt;add(function($parameterone){
        echo $parameterone;
    },"exampleFunction");
    $Foo-&gt;helloWorldFunction(); /*Output : Hello World*/
    $Foo-&gt;exampleFunction("Hello PHP"); /*Output : Hello PHP*/
?>
```
  

#

If you want to create and then immediately call a closure directly, in-line, and immediately get its return value (instead of the closure reference itself), then the proper syntax is as follows:<br><br>

```
<?php

$a = &apos;foo&apos;; $b = &apos;bar&apos;;
$test = (function() use($a,$b) { return $a . $b; })();
echo $test;

?>
```
<br><br>As for why you would want to do that? Well, that&apos;s up to you. I&apos;m sure there are some legitimate reasons. It&apos;s a pretty common pattern in some other famous scripting languages. But if you&apos;re doing this in PHP, you should think carefully and ask yourself if you really have a good reason for it, or if you should just go and re-structure your code instead. ;-)  

#

If you want to make sure that one of the parameters of your function is a Closure, you can use Type Hinting.<br>see: http://php.net/manual/en/language.oop5.typehinting.php<br><br>Example:<br>

```
<?php

class TheRoot
{
    public function poidh($param) {
        echo "TheRoot $param!";
    }   

}

class Internet
{
    # here, $my_closure must be of type object Closure
    public function run_my_closure($bar, Closure $my_closure) {
        $my_closure($bar);
    }   
}

$Internet = new Internet();
$Root = new TheRoot();

$Internet-&gt;run_my_closure($Root, function($Object) {
    $Object-&gt;poidh(42);
});

?>
```

The above code simply yields:
"TheRoot 42!"

NOTE: If you are using namespaces, make sure you give a fully qualified namespace.

print_r() of Internet::run_my_closure&apos;s $my_closure


```
<?php
Closure Object
(
    [parameter] =&gt; Array
        (
            [$Object] =&gt; 
        )

)
?>
```


var_dump() of Internet::run_my_closure&apos;s $my_closure


```
<?php
object(Closure)#3 (1) {
  ["parameter"]=&gt;
  array(1) {
    ["$Object"]=&gt;
    string(10) ""
  }
}
?>
```
  

#

When using anonymous functions as properties in Classes, note that there are three name scopes: one for constants, one for properties and one for methods. That means, you can use the same name for a constant, for a property and for a method at a time.<br><br>Since a property can be also an anonymous function as of PHP 5.3.0, an oddity arises when they share the same name, not meaning that there would be any conflict.<br><br>Consider the following example:<br><br>

```
<?php
    class MyClass {
        const member = 1;
        
        public $member;
        
        public function member () {
            return "method &apos;member&apos;";
        }
        
        public function __construct () {
            $this-&gt;member = function () {
                return "anonymous function &apos;member&apos;";
            };
        }
    }
    
    header("Content-Type: text/plain");
    
    $myObj = new MyClass();

    var_dump(MyClass::member);  // int(1)
    var_dump($myObj-&gt;member);   // object(Closure)#2 (0) {}
    var_dump($myObj-&gt;member()); // string(15) "method &apos;member&apos;"
    $myMember = $myObj-&gt;member;
    var_dump($myMember());      // string(27) "anonymous function &apos;member&apos;"
?>
```
<br><br>That means, regular method invocations work like expected and like before. The anonymous function instead, must be retrieved into a variable first (just like a property) and can only then be invoked.<br><br>Best regards,  

#

PERFORMANCE BENCHMARK 2017!<br><br>I decided to compare a single, saved closure against constantly creating the same anonymous closure on every loop iteration. And I tried 10 million loop iterations, in PHP 7.0.14 from Dec 2016. Result:<br><br>a single saved closure kept in a variable and re-used (10000000 iterations): 1.3874590396881 seconds<br><br>new anonymous closure created each time (10000000 iterations): 2.8460240364075 seconds<br><br>In other words, over the course of 10 million iterations, creating the closure again during every iteration only added a total of "1.459 seconds" to the runtime. So that means that every creation of a new anonymous closure takes about 146 nanoseconds on my 7 years old dual-core laptop. I guess PHP keeps a cached "template" for the anonymous function and therefore doesn&apos;t need much time to create a new instance of the closure!<br><br>So you do NOT have to worry about constantly re-creating your anonymous closures over and over again in tight loops! At least not as of PHP 7! There is absolutely NO need to save an instance in a variable and re-use it. And not being restricted by that is a great thing, because it means you can feel free to use anonymous functions exactly where they matter, as opposed to defining them somewhere else in the code. :-)  

#

As an alternative to gabriel&apos;s recursive construction, you may instead assign the recursive function to a variable, and use it by reference, thus:<br><br>

```
<?php
$fib = function($n)use(&amp;$fib)
{
    if($n == 0 || $n == 1) return 1;
    return $fib($n - 1) + $fib($n - 2);
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
    if($n == 0 || $n == 1) return 1;
    return $fib($n - 1) + $fib($n - 2);
};

$lie = $fib;

$fib = function($n)
{
    return 100;
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
    if($n == 0 || $n == 1) return 1;
    return $fib($n - 1) + $fib($n - 2);
};

$bark = function($f)
{
    $fib = &apos;cake&apos;;    // A totally different variable from the $fib above.
    return 2 * $f(5);
};

echo $bark($fib); // 16, twice the fifth Fibonacci number

?>
```
  

#

Anonymous functions are great for events!<br><br>

```
<?php

class Event {

  public static $events = array();
  
  public static function bind($event, $callback, $obj = null) {
    if (!self::$events[$event]) {
      self::$events[$event] = array();
    }
    
    self::$events[$event][] = ($obj === null)  ? $callback : array($obj, $callback);
  }
  
  public static function run($event) {
    if (!self::$events[$event]) return;
    
    foreach (self::$events[$event] as $callback) {
      if (call_user_func($callback) === false) break;
    }
  }

}

function hello() {
  echo "Hello from function hello()\n";
}

class Foo {
  function hello() {
    echo "Hello from foo-&gt;hello()\n";
  }
}

class Bar {
  function hello() {
    echo "Hello from Bar::hello()\n";
  }
}

$foo = new Foo();

// bind a global function to the &apos;test&apos; event
Event::bind("test", "hello");

// bind an anonymous function
Event::bind("test", function() { echo "Hello from anonymous function\n"; });

// bind an class function on an instance
Event::bind("test", "hello", $foo);

// bind a static class function
Event::bind("test", "Bar::hello");

Event::run("test");

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