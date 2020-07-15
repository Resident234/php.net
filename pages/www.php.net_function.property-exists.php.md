# property_exists



The function behaves differently depending on whether the property has been present in the class declaration, or has been added dynamically, if the variable has been unset()<br><br>

```
<?php

class TestClass {

    public $declared = null;
    
}

$testObject = new TestClass;

var_dump(property_exists("TestClass", "dynamic")); // boolean false, as expected
var_dump(property_exists($testObject, "dynamic")); // boolean false, same as above

$testObject->dynamic = null;
var_dump(property_exists($testObject, "dynamic")); // boolean true

unset($testObject->dynamic);
var_dump(property_exists($testObject, "dynamic")); // boolean false, again.

var_dump(property_exists($testObject, "declared")); // boolean true, as espected

unset($testObject->declared);
var_dump(property_exists($testObject, "declared")); // boolean true, even if has been unset()?>
```
  

#

If you are in a namespaced file, and you want to pass the class name as a string, you will have to include the full namespace for the class name - even from inside the same namespace:<br><br>&lt;?<br>namespace MyNS;<br><br>class A {<br>    public $foo;<br>}<br><br>property_exists("A", "foo");          // false<br>property_exists("\\MyNS\\A", "foo");  // true<br>?>
```
  

#



```
<?php

class Student {

    protected $_name;
    protected $_email;
    

    public function __call($name, $arguments) {
        $action = substr($name, 0, 3);
        switch ($action) {
            case 'get':
                $property = '_' . strtolower(substr($name, 3));
                if(property_exists($this,$property)){
                    return $this->{$property};
                }else{
                    echo "Undefined Property";
                }
                break;
            case 'set':
                $property = '_' . strtolower(substr($name, 3));
                if(property_exists($this,$property)){
                    $this->{$property} = $arguments[0];
                }else{
                    echo "Undefined Property";
                }
                
                break;
            default :
                return FALSE;
        }
    }

}

$s = new Student();
$s->setName('Nanhe Kumar');
$s->setEmail('nanhe.kumar@gmail.com');
echo $s->getName(); //Nanhe Kumar
echo $s->getEmail(); // nanhe.kumar@gmail.com
$s->setAge(10); //Undefined Property
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.property-exists.php)

**[To root](/README.md)**