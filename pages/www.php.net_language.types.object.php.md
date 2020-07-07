# Objects





By far the easiest and correct way to instantiate an empty generic php object that you can then modify for whatever purpose you choose:





```
<?php $genericObject = new stdClass(); ?>
```




I had the most difficult time finding this, hopefully it will help someone else!

  

#



In PHP 7 there are a few ways to create an empty object:



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
&#xA0; &#xA0; new \stdClass,
&#xA0; &#xA0; new class{},
&#xA0; &#xA0; (object)[],
]);
?>
```


Outputs: [{},{},{}]

  

#



As of PHP 5.4, we can create stdClass objects with some properties and values using the more beautiful form:



```
<?php
&#xA0; $object = (object) [
&#xA0; &#xA0; &apos;propertyOne&apos; =&gt; &apos;foo&apos;,
&#xA0; &#xA0; &apos;propertyTwo&apos; =&gt; 42,
&#xA0; ];
?>
```



  

#



Here a new updated version of &apos;stdObject&apos; class. It&apos;s very useful when extends to controller on MVC design pattern, user can create it&apos;s own class.

Hope it help you.

 

```
<?php
class stdObject {
&#xA0; &#xA0; public function __construct(array $arguments = array()) {
&#xA0; &#xA0; &#xA0; &#xA0; if (!empty($arguments)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($arguments as $property =&gt; $argument) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;{$property} = $argument;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public function __call($method, $arguments) {
&#xA0; &#xA0; &#xA0; &#xA0; $arguments = array_merge(array(&quot;stdObject&quot; =&gt; $this), $arguments); // Note: method argument 0 will always referred to the main class ($this).
&#xA0; &#xA0; &#xA0; &#xA0; if (isset($this-&gt;{$method}) &amp;&amp; is_callable($this-&gt;{$method})) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return call_user_func_array($this-&gt;{$method}, $arguments);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new Exception(&quot;Fatal error: Call to undefined method stdObject::{$method}()&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

// Usage.

$obj = new stdObject();
$obj-&gt;name = &quot;Nick&quot;;
$obj-&gt;surname = &quot;Doe&quot;;
$obj-&gt;age = 20;
$obj-&gt;adresse = null;

$obj-&gt;getInfo = function($stdObject) { // $stdObject referred to this object (stdObject).
&#xA0; &#xA0; echo $stdObject-&gt;name . &quot; &quot; . $stdObject-&gt;surname . &quot; have &quot; . $stdObject-&gt;age . &quot; yrs old. And live in &quot; . $stdObject-&gt;adresse;
};

$func = &quot;setAge&quot;;
$obj-&gt;{$func} = function($stdObject, $age) { // $age is the first parameter passed when calling this method.
&#xA0; &#xA0; $stdObject-&gt;age = $age;
};

$obj-&gt;setAge(24); // Parameter value 24 is passing to the $age argument in method &apos;setAge()&apos;.

// Create dynamic method. Here i&apos;m generating getter and setter dynimically
// Beware: Method name are case sensitive.
foreach ($obj as $func_name =&gt; $value) {
&#xA0; &#xA0; if (!$value instanceOf Closure) {

&#xA0; &#xA0; &#xA0; &#xA0; $obj-&gt;{&quot;set&quot; . ucfirst($func_name)} = function($stdObject, $value) use ($func_name) {&#xA0; // Note: you can also use keyword &apos;use&apos; to bind parent variables.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stdObject-&gt;{$func_name} = $value;
&#xA0; &#xA0; &#xA0; &#xA0; };

&#xA0; &#xA0; &#xA0; &#xA0; $obj-&gt;{&quot;get&quot; . ucfirst($func_name)} = function($stdObject) use ($func_name) {&#xA0; // Note: you can also use keyword &apos;use&apos; to bind parent variables.
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $stdObject-&gt;{$func_name};
&#xA0; &#xA0; &#xA0; &#xA0; };

&#xA0; &#xA0; }
}

$obj-&gt;setName(&quot;John&quot;);
$obj-&gt;setAdresse(&quot;Boston&quot;);

$obj-&gt;getInfo();
?>
```



  

#



&lt;!--Example shows how to convert array to stdClass Object and how to access its value for display --&gt;


```
<?php 
$num = array(&quot;Garha&quot;,&quot;sitamarhi&quot;,&quot;canada&quot;,&quot;patna&quot;); //create an array
$obj = (object)$num; //change array to stdClass object 

echo &quot;&lt;pre&gt;&quot;;
print_r($obj); //stdClass Object created by casting of array 

$newobj = new stdClass();//create a new 
$newobj-&gt;name = &quot;India&quot;;
$newobj-&gt;work = &quot;Development&quot;;
$newobj-&gt;address=&quot;patna&quot;;

$new = (array)$newobj;//convert stdClass to array
echo &quot;&lt;pre&gt;&quot;;
print_r($new); //print new object

##How deals with Associative Array

$test = [Details=&gt;[&apos;name&apos;,&apos;roll number&apos;,&apos;college&apos;,&apos;mobile&apos;],values=&gt;[&apos;Naman Kumar&apos;,&apos;100790310868&apos;,&apos;Pune college&apos;,&apos;9988707202&apos;]];
$val = json_decode(json_encode($test),false);//convert array into stdClass object

echo &quot;&lt;pre&gt;&quot;;
print_r($val);

echo ((is_array($val) == true ?&#xA0; 1 : 0 ) == 1 ? &quot;array&quot; : &quot;not an array&quot; ).&quot;&lt;/br&gt;&quot;; // check whether it is array or not
echo ((is_object($val) == true ?&#xA0; 1 : 0 ) == 1 ? &quot;object&quot; : &quot;not an object&quot; );//check whether it is object or not 
?>
```



  

#



CAUTION:
&quot;Arrays convert to an object with properties named by keys, and corresponding values&quot;.

This is ALWAYS true, which means that even numeric keys are accepted when converting.
But the resulting properties cannot be accessed, since they don&apos;t match the variables naming rules.

So this:


```
<?php
$x = (object) array(&apos;a&apos;=&gt;&apos;A&apos;, &apos;b&apos;=&gt;&apos;B&apos;, &apos;C&apos;);
echo &apos;&lt;pre&gt;&apos;.print_r($x, true).&apos;&lt;/pre&gt;&apos;;
?>
```

works and displays:
stdClass Object
(
&#xA0; &#xA0; [a] =&gt; A
&#xA0; &#xA0; [b] =&gt; B
&#xA0; &#xA0; [0] =&gt; C
)

But this:


```
<?php
echo &apos;&lt;br /&gt;&apos;.$x-&gt;a;
echo &apos;&lt;br /&gt;&apos;.$x-&gt;b;
echo &apos;&lt;br /&gt;&apos;.$x-&gt;{0}; # (don&apos;t use $x-&gt;0, which is obviously a syntax error)
?>
```

fails and displays:
A
B
Notice: Undefined property: stdClass::$0 in...

  

#

[Official documentation page](https://www.php.net/manual/en/language.types.object.php)

**[To root](/README.md)**