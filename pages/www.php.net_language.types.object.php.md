# Objects



By far the easiest and correct way to instantiate an empty generic php object that you can then modify for whatever purpose you choose:<br><br>

```
<?php $genericObject = new stdClass(); ?>
```
<br><br>I had the most difficult time finding this, hopefully it will help someone else!  

#

In PHP 7 there are a few ways to create an empty object:<br><br>

```
<?php
$obj1 = new \stdClass; // Instantiate stdClass object
$obj2 = new class{}; // Instantiate anonymous class
$obj3 = (object)[]; // Cast empty array to object

var_dump($obj1); // object(stdClass)#1 (0) {}
var_dump($obj2); // object(class@anonymous)#2 (0) {}
var_dump($obj3); // object(stdClass)#3 (0) {}
?>
```


$obj1 and $obj3 are the same type, but $obj1 !== $obj3. Also, all three will json_encode() to a simple JS object {}:



```
<?php
echo json_encode([
    new \stdClass,
    new class{},
    (object)[],
]);
?>
```
<br><br>Outputs: [{},{},{}]  

#

As of PHP 5.4, we can create stdClass objects with some properties and values using the more beautiful form:<br><br>

```
<?php
  $object = (object) [
    &apos;propertyOne&apos; =&gt; &apos;foo&apos;,
    &apos;propertyTwo&apos; =&gt; 42,
  ];
?>
```
  

#

Here a new updated version of &apos;stdObject&apos; class. It&apos;s very useful when extends to controller on MVC design pattern, user can create it&apos;s own class.<br><br>Hope it help you.<br><br> 

```
<?php
class stdObject {
    public function __construct(array $arguments = array()) {
        if (!empty($arguments)) {
            foreach ($arguments as $property =&gt; $argument) {
                $this-&gt;{$property} = $argument;
            }
        }
    }

    public function __call($method, $arguments) {
        $arguments = array_merge(array("stdObject" =&gt; $this), $arguments); // Note: method argument 0 will always referred to the main class ($this).
        if (isset($this-&gt;{$method}) &amp;&amp; is_callable($this-&gt;{$method})) {
            return call_user_func_array($this-&gt;{$method}, $arguments);
        } else {
            throw new Exception("Fatal error: Call to undefined method stdObject::{$method}()");
        }
    }
}

// Usage.

$obj = new stdObject();
$obj-&gt;name = "Nick";
$obj-&gt;surname = "Doe";
$obj-&gt;age = 20;
$obj-&gt;adresse = null;

$obj-&gt;getInfo = function($stdObject) { // $stdObject referred to this object (stdObject).
    echo $stdObject-&gt;name . " " . $stdObject-&gt;surname . " have " . $stdObject-&gt;age . " yrs old. And live in " . $stdObject-&gt;adresse;
};

$func = "setAge";
$obj-&gt;{$func} = function($stdObject, $age) { // $age is the first parameter passed when calling this method.
    $stdObject-&gt;age = $age;
};

$obj-&gt;setAge(24); // Parameter value 24 is passing to the $age argument in method &apos;setAge()&apos;.

// Create dynamic method. Here i&apos;m generating getter and setter dynimically
// Beware: Method name are case sensitive.
foreach ($obj as $func_name =&gt; $value) {
    if (!$value instanceOf Closure) {

        $obj-&gt;{"set" . ucfirst($func_name)} = function($stdObject, $value) use ($func_name) {  // Note: you can also use keyword &apos;use&apos; to bind parent variables.
            $stdObject-&gt;{$func_name} = $value;
        };

        $obj-&gt;{"get" . ucfirst($func_name)} = function($stdObject) use ($func_name) {  // Note: you can also use keyword &apos;use&apos; to bind parent variables.
            return $stdObject-&gt;{$func_name};
        };

    }
}

$obj-&gt;setName("John");
$obj-&gt;setAdresse("Boston");

$obj-&gt;getInfo();
?>
```
  

#

&lt;!--Example shows how to convert array to stdClass Object and how to access its value for display --&gt;<br>

```
<?php 
$num = array("Garha","sitamarhi","canada","patna"); //create an array
$obj = (object)$num; //change array to stdClass object 

echo "&lt;pre&gt;";
print_r($obj); //stdClass Object created by casting of array 

$newobj = new stdClass();//create a new 
$newobj-&gt;name = "India";
$newobj-&gt;work = "Development";
$newobj-&gt;address="patna";

$new = (array)$newobj;//convert stdClass to array
echo "&lt;pre&gt;";
print_r($new); //print new object

##How deals with Associative Array

$test = [Details=&gt;[&apos;name&apos;,&apos;roll number&apos;,&apos;college&apos;,&apos;mobile&apos;],values=&gt;[&apos;Naman Kumar&apos;,&apos;100790310868&apos;,&apos;Pune college&apos;,&apos;9988707202&apos;]];
$val = json_decode(json_encode($test),false);//convert array into stdClass object

echo "&lt;pre&gt;";
print_r($val);

echo ((is_array($val) == true ?  1 : 0 ) == 1 ? "array" : "not an array" )."&lt;/br&gt;"; // check whether it is array or not
echo ((is_object($val) == true ?  1 : 0 ) == 1 ? "object" : "not an object" );//check whether it is object or not 
?>
```
  

#

CAUTION:<br>"Arrays convert to an object with properties named by keys, and corresponding values".<br><br>This is ALWAYS true, which means that even numeric keys are accepted when converting.<br>But the resulting properties cannot be accessed, since they don&apos;t match the variables naming rules.<br><br>So this:<br>

```
<?php
$x = (object) array(&apos;a&apos;=&gt;&apos;A&apos;, &apos;b&apos;=&gt;&apos;B&apos;, &apos;C&apos;);
echo &apos;&lt;pre&gt;&apos;.print_r($x, true).&apos;&lt;/pre&gt;&apos;;
?>
```

works and displays:
stdClass Object
(
    [a] =&gt; A
    [b] =&gt; B
    [0] =&gt; C
)

But this:


```
<?php
echo &apos;&lt;br /&gt;&apos;.$x-&gt;a;
echo &apos;&lt;br /&gt;&apos;.$x-&gt;b;
echo &apos;&lt;br /&gt;&apos;.$x-&gt;{0}; # (don&apos;t use $x-&gt;0, which is obviously a syntax error)
?>
```
<br>fails and displays:<br>A<br>B<br>Notice: Undefined property: stdClass::$0 in...  

#

[Official documentation page](https://www.php.net/manual/en/language.types.object.php)

**[To root](/README.md)**