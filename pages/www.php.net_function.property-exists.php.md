# property_exists



The function behaves differently depending on whether the property has been present in the class declaration, or has been added dynamically, if the variable has been unset()<br><br>

```
<?php<br><br>class TestClass {<br><br>    public $declared = null;<br>    <br>}<br><br>$testObject = new TestClass;<br><br>var_dump(property_exists("TestClass", "dynamic")); // boolean false, as expected<br>var_dump(property_exists($testObject, "dynamic")); // boolean false, same as above<br><br>$testObject-&gt;dynamic = null;<br>var_dump(property_exists($testObject, "dynamic")); // boolean true<br><br>unset($testObject-&gt;dynamic);<br>var_dump(property_exists($testObject, "dynamic")); // boolean false, again.<br><br>var_dump(property_exists($testObject, "declared")); // boolean true, as espected<br><br>unset($testObject-&gt;declared);<br>var_dump(property_exists($testObject, "declared")); // boolean true, even if has been unset()  

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
            case &apos;get&apos;:
                $property = &apos;_&apos; . strtolower(substr($name, 3));
                if(property_exists($this,$property)){
                    return $this-&gt;{$property};
                }else{
                    echo "Undefined Property";
                }
                break;
            case &apos;set&apos;:
                $property = &apos;_&apos; . strtolower(substr($name, 3));
                if(property_exists($this,$property)){
                    $this-&gt;{$property} = $arguments[0];
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
$s-&gt;setName(&apos;Nanhe Kumar&apos;);
$s-&gt;setEmail(&apos;nanhe.kumar@gmail.com&apos;);
echo $s-&gt;getName(); //Nanhe Kumar
echo $s-&gt;getEmail(); // nanhe.kumar@gmail.com
$s-&gt;setAge(10); //Undefined Property
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.property-exists.php)

**[To root](/README.md)**