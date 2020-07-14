# Class Abstraction



Just one more time, in the simplest terms possible:<br><br>An Interface is like a protocol. It doesn&apos;t designate the behavior of the object; it designates how your code tells that object to act. An interface would be like the English Language: defining an interface defines how your code communicates with any object implementing that interface.<br><br>An interface is always an agreement or a promise. When a class says "I implement interface Y", it is saying "I promise to have the same public methods that any object with interface Y has".<br><br>On the other hand, an Abstract Class is like a partially built class. It is much like a document with blanks to fill in. It might be using English, but that isn&apos;t as important as the fact that some of the document is already written.<br><br>An abstract class is the foundation for another object. When a class says "I extend abstract class Y", it is saying "I use some methods or properties already defined in this other class named Y".<br><br>So, consider the following PHP:<br>

```
<?php
class X implements Y { } // this is saying that "X" agrees to speak language "Y" with your code.

class X extends Y { } // this is saying that "X" is going to complete the partial class "Y".
?>
```
<br><br>You would have your class implement a particular interface if you were distributing a class to be used by other people. The interface is an agreement to have a specific set of public methods for your class.<br><br>You would have your class extend an abstract class if you (or someone else) wrote a class that already had some methods written that you want to use in your new class.<br><br>These concepts, while easy to confuse, are specifically different and distinct. For all intents and purposes, if you&apos;re the only user of any of your classes, you don&apos;t need to implement interfaces.  

#

Here&apos;s an example that helped me with understanding abstract classes. It&apos;s just a very simple way of explaining it (in my opinion). Lets say we have the following code:<br><br>

```
<?php
class Fruit {
    private $color;
    
    public function eat() {
        //chew
    }
    
    public function setColor($c) {
        $this->color = $c;
    }
}

class Apple extends Fruit {
    public function eat() {
        //chew until core
    }
}

class Orange extends Fruit {
    public function eat() {
        //peel
        //chew
    }
}
?>
```


Now I give you an apple and you eat it.



```
<?php
$apple = new Apple();
$apple->eat();
?>
```


What does it taste like? It tastes like an apple. Now I give you a fruit.



```
<?php
$fruit = new Fruit();
$fruit->eat();
?>
```


What does that taste like??? Well, it doesn't make much sense, so you shouldn't be able to do that. This is accomplished by making the Fruit class abstract as well as the eat method inside of it.



```
<?php
abstract class Fruit {
    private $color;
    
    abstract public function eat();
    
    public function setColor($c) {
        $this->color = $c;
    }
}
?>
```
<br><br>Now just think about a Database class where MySQL and PostgreSQL extend it. Also, a note. An abstract class is just like an interface, but you can define methods in an abstract class whereas in an interface they are all abstract.  

#

Abstraction and interfaces are two very different tools. The are as close as hammers and drills. Abstract classes may have implemented methods, whereas interfaces have no implementation in themselves.<br><br>Abstract classes that declare all their methods as abstract are not interfaces with different names. One can implement multiple interfaces, but not extend multiple classes (or abstract classes).<br><br>The use of abstraction vs interfaces is problem specific and the choice is made during the design of software, not its implementation. In the same project you may as well offer an interface and a base (probably abstract) class as a reference that implements the interface. Why would you do that?<br><br>Let us assume that we want to build a system that calls different services, which in turn have actions. Normally, we could offer a method called execute that accepts the name of the action as a parameter and executes the action.<br><br>We want to make sure that classes can actually define their own ways of executing actions. So we create an interface IService that has the execute method. Well, in most of your cases, you will be copying and pasting the exact same code for execute.<br><br>We can create a reference implemention for a class named Service and implement the execute method. So, no more copying and pasting for your other classes! But what if you want to extend MySLLi?? You can implement the interface (copy-paste probably), and there you are, again with a service. Abstraction can be included in the class for initialisation code, which cannot be predefined for every class that you will write.<br><br>Hope this is not too mind-boggling and helps someone. Cheers,<br>Alexios Tsiaparas  

#

The documentation says: "It is not allowed to create an instance of a class that has been defined as abstract.". It only means you cannot initialize an object from an abstract class. Invoking static method of abstract class is still feasible. For example:<br>

```
<?php
abstract class Foo
{
    static function bar()
    {
        echo "test\n";
    }
}

Foo::bar();
?>
```
  

#

This example will hopefully help you see how abstract works, how interfaces work, and how they can work together. This example will also work/compile on PHP7, the others were typed live in the form and may work but the last one was made/tested for real:<br><br>

```
<?php

const &#xB6; = PHP_EOL;

// Define things a product *has* to be able to do (has to implement)
interface productInterface {
    public function doSell();
    public function doBuy();
}

// Define our default abstraction
abstract class defaultProductAbstraction implements productInterface {
    private $_bought = false;
    private $_sold = false;
    abstract public function doMore();
    public function doSell() {
        /* the default implementation */
        $this->_sold = true;
        echo "defaultProductAbstraction doSell: {$this->_sold}".&#xB6;;
    }
    public function doBuy() {
        $this->_bought = true;
        echo "defaultProductAbstraction doBuy: {$this->_bought}".&#xB6;;
    }
}

class defaultProductImplementation extends defaultProductAbstraction {
    public function doMore() {
        echo "defaultProductImplementation doMore()".&#xB6;;
    }
}

class myProductImplementation extends defaultProductAbstraction {
    public function doMore() {
        echo "myProductImplementation doMore() does more!".&#xB6;;
    }
    public function doBuy() {
        echo "myProductImplementation's doBuy() and also my parent's dubai()".&#xB6;;
        parent::doBuy();
    }
}

class myProduct extends defaultProductImplementation {
    private $_bought=true;
    public function __construct() {
        var_dump($this->_bought);
    }
    public function doBuy () {
        /* non-default doBuy implementation */
        $this->_bought = true;
        echo "myProduct overrides the defaultProductImplementation's doBuy() here {$this->_bought}".&#xB6;;
    }
}

class myOtherProduct extends myProductImplementation {
    public function doBuy() {
        echo "myOtherProduct overrides myProductImplementations doBuy() here but still calls parent too".&#xB6;;
        parent::doBuy();
    }
}

echo "new myProduct()".&#xB6;;
$product = new myProduct();

$product->doBuy();
$product->doSell();
$product->doMore();

echo &#xB6;."new defaultProductImplementation()".&#xB6;;

$newProduct = new defaultProductImplementation();
$newProduct->doBuy();
$newProduct->doSell();
$newProduct->doMore();

echo &#xB6;."new myProductImplementation".&#xB6;;
$lastProduct = new myProductImplementation();
$lastProduct->doBuy();
$lastProduct->doSell();
$lastProduct->doMore();

echo &#xB6;."new myOtherProduct".&#xB6;;
$anotherNewProduct = new myOtherProduct();
$anotherNewProduct->doBuy();
$anotherNewProduct->doSell();
$anotherNewProduct->doMore();
?>
```


Will result in:


```
<?php
/*
new myProduct()
bool(true)
myProduct overrides the defaultProductImplementation's doBuy() here 1
defaultProductAbstraction doSell: 1
defaultProductImplementation doMore()

new defaultProductImplementation()
defaultProductAbstraction doBuy: 1
defaultProductAbstraction doSell: 1
defaultProductImplementation doMore()

new myProductImplementation
myProductImplementation's doBuy() and also my parent's dubai()
defaultProductAbstraction doBuy: 1
defaultProductAbstraction doSell: 1
myProductImplementation doMore() does more!

new myOtherProduct
myOtherProduct overrides myProductImplementations doBuy() here but still calls parent too
myProductImplementation's doBuy() and also my parent's dubai()
defaultProductAbstraction doBuy: 1
defaultProductAbstraction doSell: 1
myProductImplementation doMore() does more!

*/
?>
```
  

#

Ok...the docs are a bit vague when it comes to an abstract class extending another abstract class.  An abstract class that extends another abstract class doesn&apos;t need to define the abstract methods from the parent class.  In other words, this causes an error:<br><br>

```
<?php
abstract class class1 {
  abstract public function someFunc();
}
abstract class class2 extends class1 {
  abstract public function someFunc();
}
?>
```


Error: Fatal error: Can't inherit abstract function class1::someFunc() (previously declared abstract in class2) in /home/sneakyimp/public/chump.php on line 7

However this does not:



```
<?php
abstract class class1 {
  abstract public function someFunc();
}
abstract class class2 extends class1 {
}
?>
```
<br><br>An abstract class that extends an abstract class can pass the buck to its child classes when it comes to implementing the abstract methods of its parent abstract class.  

#

Incidentally, abstract classes do not need to be base classes:<br><br>

```
<?php
class Foo {
    public function sneeze() { echo 'achoooo'; }
}

abstract class Bar extends Foo {
    public abstract function hiccup();
}

class Baz extends Bar {
    public function hiccup() { echo 'hiccup!'; }
}

$baz = new Baz();
$baz->sneeze();
$baz->hiccup();
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.abstract.php)

**[To root](/README.md)**