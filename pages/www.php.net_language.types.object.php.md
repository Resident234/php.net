# Objects



By far the easiest and correct way to instantiate an empty generic php object that you can then modify for whatever purpose you choose:<br><br>

```
<?php $genericObject = new stdClass(); ?>
```
<br><br>I had the most difficult time finding this, hopefully it will help someone else!  

---

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

---

As of PHP 5.4, we can create stdClass objects with some properties and values using the more beautiful form:<br><br>

```
<?php
  $object = (object) [
    'propertyOne' => 'foo',
    'propertyTwo' => 42,
  ];
?>
```
  

---

Here a new updated version of &apos;stdObject&apos; class. It&apos;s very useful when extends to controller on MVC design pattern, user can create it&apos;s own class.<br><br>Hope it help you.<br><br> 

```
<?php
class stdObject {
    public function __construct(array $arguments = array()) {
        if (!empty($arguments)) {
            foreach ($arguments as $property => $argument) {
                $this->{$property} = $argument;
            }
        }
    }

    public function __call($method, $arguments) {
        $arguments = array_merge(array("stdObject" => $this), $arguments); // Note: method argument 0 will always referred to the main class ($this).
        if (isset($this->{$method}) &amp;&amp; is_callable($this->{$method})) {
            return call_user_func_array($this->{$method}, $arguments);
        } else {
            throw new Exception("Fatal error: Call to undefined method stdObject::{$method}()");
        }
    }
}

// Usage.

$obj = new stdObject();
$obj->name = "Nick";
$obj->surname = "Doe";
$obj->age = 20;
$obj->adresse = null;

$obj->getInfo = function($stdObject) { // $stdObject referred to this object (stdObject).
    echo $stdObject->name . " " . $stdObject->surname . " have " . $stdObject->age . " yrs old. And live in " . $stdObject->adresse;
};

$func = "setAge";
$obj->{$func} = function($stdObject, $age) { // $age is the first parameter passed when calling this method.
    $stdObject->age = $age;
};

$obj->setAge(24); // Parameter value 24 is passing to the $age argument in method 'setAge()'.

// Create dynamic method. Here i'm generating getter and setter dynimically
// Beware: Method name are case sensitive.
foreach ($obj as $func_name => $value) {
    if (!$value instanceOf Closure) {

        $obj->{"set" . ucfirst($func_name)} = function($stdObject, $value) use ($func_name) {  // Note: you can also use keyword 'use' to bind parent variables.
            $stdObject->{$func_name} = $value;
        };

        $obj->{"get" . ucfirst($func_name)} = function($stdObject) use ($func_name) {  // Note: you can also use keyword 'use' to bind parent variables.
            return $stdObject->{$func_name};
        };

    }
}

$obj->setName("John");
$obj->setAdresse("Boston");

$obj->getInfo();
?>
```
  

---

&lt;!--Example shows how to convert array to stdClass Object and how to access its value for display --&gt;<br>

```
<?php 
$num = array("Garha","sitamarhi","canada","patna"); //create an array
$obj = (object)$num; //change array to stdClass object 

echo "<pre>";
print_r($obj); //stdClass Object created by casting of array 

$newobj = new stdClass();//create a new 
$newobj->name = "India";
$newobj->work = "Development";
$newobj->address="patna";

$new = (array)$newobj;//convert stdClass to array
echo "<pre>";
print_r($new); //print new object

##How deals with Associative Array

$test = [Details=>['name','roll number','college','mobile'],values=>['Naman Kumar','100790310868','Pune college','9988707202']];
$val = json_decode(json_encode($test),false);//convert array into stdClass object

echo "<pre>";
print_r($val);

echo ((is_array($val) == true ?  1 : 0 ) == 1 ? "array" : "not an array" )."</br>"; // check whether it is array or not
echo ((is_object($val) == true ?  1 : 0 ) == 1 ? "object" : "not an object" );//check whether it is object or not 
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.types.object.php)

**[To root](/README.md)**