# The Reflection class



Here is a code snippet for some of us who are just beginning with reflection. I have a simple class below with two properties and two methods. We will use reflection classes to populate the properties dynamically and then print them:<br><br>

```
<?php

class A
{
    public $one = '';
    public $two = '';
    
    //Constructor
    public function __construct()
    {
        //Constructor
    }
    
    //print variable one
    public function echoOne()
    {
        echo $this->one."\n";
    }

    //print variable two    
    public function echoTwo()
    {
        echo $this->two."\n";
    }
}

//Instantiate the object
$a = new A();

//Instantiate the reflection object
$reflector = new ReflectionClass('A');

//Now get all the properties from class A in to $properties array
$properties = $reflector->getProperties();

$i =1;
//Now go through the $properties array and populate each property
foreach($properties as $property)
{
    //Populating properties
    $a->{$property->getName()}=$i;
    //Invoking the method to print what was populated
    $a->{"echo".ucfirst($property->getName())}()."\n";
    
    $i++;
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.reflection.php)

**[To root](/README.md)**