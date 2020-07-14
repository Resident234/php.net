# Object Inheritance



Here is some clarification about PHP inheritance &#x2013; there is a lot of bad information on the net.  PHP does support Multi-level inheritance.  (I tested it using version 5.2.9).  It does not support multiple inheritance.<br> <br>This means that you cannot have one class extend 2 other classes (see the extends keyword).  However, you can have one class extend another, which extends another, and so on. <br> <br>Example:<br> <br>

```
<?php
class A {
        // more code here
}
 
class B extends A {
        // more code here
}
 
class C extends B {
        // more code here
}
 
 
$someObj = new A();  // no problems
$someOtherObj = new B(); // no problems
$lastObj = new C(); // still no problems
 
?>
```
  

#

I think the best way for beginners to understand inheritance is through a real example so here is a simple example I can gave to you <br><br>

```
<?php

class Person
{
    public $name;
    protected $age;
    private $phone;

    public function talk(){
        //Do stuff here
    }

    protected function walk(){
        //Do stuff here
    }

    private function swim(){
        //Do stuff here
    }
}

class Tom extends Person
{
    /*Since Tom class extends Person class this means 
        that class Tom is a child class and class person is 
        the parent class and child class will inherit all public 
        and protected members(properties and methods) from
        the parent class*/

     /*So class Tom will have these properties and methods*/

     //public $name;
     //protected $age;
     //public function talk(){}
     //protected function walk(){}

     //but it will not inherit the private members 
     //this is all what Object inheritance means
}?>
```
  

#

The Idea that multiple inheritence is not supported is correct but with tratits this can be reviewed.<br><br>for e.g.<br> <br>

```
<?php
trait  custom
{
     public function hello()
     {
          echo "hello";
     }
}

trait custom2
{
       public function hello()
       {
            echo "hello2";
       }
}

class inheritsCustom
{
        use custom, custom2
        {
              custom2::hello insteadof custom;
        }
}

$obj = new inheritsCustom();
$obj->hello();
?>
```
  

#

I was recently extending a PEAR class when I encountered a situation where I wanted to call a constructor two levels up the class hierarchy, ignoring the immediate parent.  In such a case, you need to explicitly reference the class name using the :: operator.<br><br>Fortunately, just like using the &apos;parent&apos; keyword PHP correctly recognizes that you are calling the function from a protected context inside the object&apos;s class hierarchy.<br><br>E.g:<br><br>

```
<?php
class foo
{
  public function something()
  {
    echo __CLASS__; // foo
    var_dump($this);
  }
}

class foo_bar extends foo
{
  public function something()
  {
    echo __CLASS__; // foo_bar
    var_dump($this);
  }
}

class foo_bar_baz extends foo_bar
{
  public function something()
  {
    echo __CLASS__; // foo_bar_baz
    var_dump($this);
  }

  public function call()
  {
    echo self::something(); // self
    echo parent::something(); // parent
    echo foo::something(); // grandparent
  }
}

error_reporting(-1);

$obj = new foo_bar_baz();
$obj->call();

// Output similar to:
// foo_bar_baz
// object(foo_bar_baz)[1]
// foo_bar
// object(foo_bar_baz)[1]
// foo
// object(foo_bar_baz)[1]

?>
```
  

#

You can force a class to be strictly an inheritable class by using the "abstract" keyword. When you define a class with abstract, any attempt to instantiate a separate instance of it will result in a fatal error. This is useful for situations like a base class where it would be inherited by multiple child classes yet you want to restrict the ability to instantiate it by itself.<br><br>Example........<br><br>

```
<?php

abstract class Cheese
{
      //can ONLY be inherited by another class
}

class Cheddar extends Cheese
{
}

$dinner = new Cheese; //fatal error
$lunch = new Cheddar; //works!

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.inheritance.php)

**[To root](/README.md)**