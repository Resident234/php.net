# INSTANCEOF



When checking instanceof against a subclass of the class in question, it will return true.<br><br>

```
<?php

class Foo {

    public $foobar = &apos;Foo&apos;;
    
    public function test() {
        echo $this-&gt;foobar . "\n";
    }

}

class Bar extends Foo {

    public $foobar = &apos;Bar&apos;;

}

$a = new Foo();
$b = new Bar();

echo "use of test() method\n";
$a-&gt;test();
$b-&gt;test();

echo "instanceof Foo\n";
var_dump($a instanceof Foo); // TRUE
var_dump($b instanceof Foo); // TRUE

echo "instanceof Bar\n";
var_dump($a instanceof Bar); // FALSE
var_dump($b instanceof Bar); // TRUE

echo "subclass of Foo\n";
var_dump(is_subclass_of($a, &apos;Foo&apos;)); // FALSE
var_dump(is_subclass_of($b, &apos;Foo&apos;)); // TRUE

echo "subclass of Bar\n";
var_dump(is_subclass_of($a, &apos;Bar&apos;)); // FALSE
var_dump(is_subclass_of($b, &apos;Bar&apos;)); // FALSE

?>
```
<br><br>Result (CLI, 5.4.4):<br><br>use of test() method<br>Foo<br>Bar<br>instanceof Foo<br>bool(true)<br>bool(true)<br>instanceof Bar<br>bool(false)<br>bool(true)<br>subclass of Foo<br>bool(false)<br>bool(true)<br>subclass of Bar<br>bool(false)<br>bool(false)  

#

Please note, that you get no warnings on non-existent classes:<br><br>

```
<?php<br>class A() {<br>}<br><br>$a = new A();<br><br>$exists = ($a instanceof A); //TRUE<br>$exists = ($a instanceof NonExistentClass); //FALSE  

#

I&apos;m commenting here because the note from "admin at torntech" is incomplete. You can perfectly replace "is_a()" function with "instanceof" operator. However you must use a variable to store the class name, otherwise you will get a Parse error:<br><br>

```
<?php
$object = new \stdClass();
$class_name = &apos;\stdClass&apos;;

var_dump($object instanceof $class_name);    // bool(true)
var_dump($object instanceof &apos;\stdClass&apos;);    // Parse error: syntax error, unexpected &apos;&apos;\stdClass&apos;&apos; (T_CONSTANT_ENCAPSED_STRING)
?>
```
<br><br>Please go to Type Operators page for more details about "instanceof": http://php.net/manual/en/language.operators.type.php  

#

When checking instanceof against a class that implements the class in question, it will return true.<br><br>

```
<?php

interface ExampleInterface
{
    public function interfaceMethod();

}

class ExampleClass implements ExampleInterface
{
    public function interfaceMethod()
    {
        return &apos;Hello World!&apos;;
    }
}

$exampleInstance = new ExampleClass();

if($exampleInstance instanceof ExampleInterface)
    echo &apos;Yes, it is&apos;;
else
    echo &apos;No, it is not&apos;;

?>
```
<br><br>The result:<br><br>Yes, it is  

#

[Official documentation page](https://www.php.net/manual/en/internals2.opcodes.instanceof.php)

**[To root](/README.md)**