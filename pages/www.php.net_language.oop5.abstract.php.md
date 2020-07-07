# Class Abstraction





Just one more time, in the simplest terms possible:

An Interface is like a protocol. It doesn&apos;t designate the behavior of the object; it designates how your code tells that object to act. An interface would be like the English Language: defining an interface defines how your code communicates with any object implementing that interface.

An interface is always an agreement or a promise. When a class says &quot;I implement interface Y&quot;, it is saying &quot;I promise to have the same public methods that any object with interface Y has&quot;.

On the other hand, an Abstract Class is like a partially built class. It is much like a document with blanks to fill in. It might be using English, but that isn&apos;t as important as the fact that some of the document is already written.

An abstract class is the foundation for another object. When a class says &quot;I extend abstract class Y&quot;, it is saying &quot;I use some methods or properties already defined in this other class named Y&quot;.

So, consider the following PHP:


```
<?php
class X implements Y { } // this is saying that &quot;X&quot; agrees to speak language &quot;Y&quot; with your code.

class X extends Y { } // this is saying that &quot;X&quot; is going to complete the partial class &quot;Y&quot;.
?>
```


You would have your class implement a particular interface if you were distributing a class to be used by other people. The interface is an agreement to have a specific set of public methods for your class.

You would have your class extend an abstract class if you (or someone else) wrote a class that already had some methods written that you want to use in your new class.

These concepts, while easy to confuse, are specifically different and distinct. For all intents and purposes, if you&apos;re the only user of any of your classes, you don&apos;t need to implement interfaces.

  

#



Here&apos;s an example that helped me with understanding abstract classes. It&apos;s just a very simple way of explaining it (in my opinion). Lets say we have the following code:





```
<?php

class Fruit {

&#xA0; &#xA0; private $color;

&#xA0; &#xA0; 

&#xA0; &#xA0; public function eat() {

&#xA0; &#xA0; &#xA0; &#xA0; //chew

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; public function setColor($c) {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;color = $c;

&#xA0; &#xA0; }

}



class Apple extends Fruit {

&#xA0; &#xA0; public function eat() {

&#xA0; &#xA0; &#xA0; &#xA0; //chew until core

&#xA0; &#xA0; }

}



class Orange extends Fruit {

&#xA0; &#xA0; public function eat() {

&#xA0; &#xA0; &#xA0; &#xA0; //peel

&#xA0; &#xA0; &#xA0; &#xA0; //chew

&#xA0; &#xA0; }

}

?>
```




Now I give you an apple and you eat it.





```
<?php

$apple = new Apple();

$apple-&gt;eat();

?>
```




What does it taste like? It tastes like an apple. Now I give you a fruit.





```
<?php

$fruit = new Fruit();

$fruit-&gt;eat();

?>
```




What does that taste like??? Well, it doesn&apos;t make much sense, so you shouldn&apos;t be able to do that. This is accomplished by making the Fruit class abstract as well as the eat method inside of it.





```
<?php

abstract class Fruit {

&#xA0; &#xA0; private $color;

&#xA0; &#xA0; 

&#xA0; &#xA0; abstract public function eat();

&#xA0; &#xA0; 

&#xA0; &#xA0; public function setColor($c) {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;color = $c;

&#xA0; &#xA0; }

}

?>
```




Now just think about a Database class where MySQL and PostgreSQL extend it. Also, a note. An abstract class is just like an interface, but you can define methods in an abstract class whereas in an interface they are all abstract.

  

#



Abstraction and interfaces are two very different tools. The are as close as hammers and drills. Abstract classes may have implemented methods, whereas interfaces have no implementation in themselves.

Abstract classes that declare all their methods as abstract are not interfaces with different names. One can implement multiple interfaces, but not extend multiple classes (or abstract classes).

The use of abstraction vs interfaces is problem specific and the choice is made during the design of software, not its implementation. In the same project you may as well offer an interface and a base (probably abstract) class as a reference that implements the interface. Why would you do that?

Let us assume that we want to build a system that calls different services, which in turn have actions. Normally, we could offer a method called execute that accepts the name of the action as a parameter and executes the action.

We want to make sure that classes can actually define their own ways of executing actions. So we create an interface IService that has the execute method. Well, in most of your cases, you will be copying and pasting the exact same code for execute.

We can create a reference implemention for a class named Service and implement the execute method. So, no more copying and pasting for your other classes! But what if you want to extend MySLLi?? You can implement the interface (copy-paste probably), and there you are, again with a service. Abstraction can be included in the class for initialisation code, which cannot be predefined for every class that you will write.

Hope this is not too mind-boggling and helps someone. Cheers,
Alexios Tsiaparas

  

#



The documentation says: &quot;It is not allowed to create an instance of a class that has been defined as abstract.&quot;. It only means you cannot initialize an object from an abstract class. Invoking static method of abstract class is still feasible. For example:


```
<?php
abstract class Foo
{
&#xA0; &#xA0; static function bar()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;test\n&quot;;
&#xA0; &#xA0; }
}

Foo::bar();
?>
```



  

#



This example will hopefully help you see how abstract works, how interfaces work, and how they can work together. This example will also work/compile on PHP7, the others were typed live in the form and may work but the last one was made/tested for real:



```
<?php

const &#xB6; = PHP_EOL;

// Define things a product *has* to be able to do (has to implement)
interface productInterface {
&#xA0; &#xA0; public function doSell();
&#xA0; &#xA0; public function doBuy();
}

// Define our default abstraction
abstract class defaultProductAbstraction implements productInterface {
&#xA0; &#xA0; private $_bought = false;
&#xA0; &#xA0; private $_sold = false;
&#xA0; &#xA0; abstract public function doMore();
&#xA0; &#xA0; public function doSell() {
&#xA0; &#xA0; &#xA0; &#xA0; /* the default implementation */
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;_sold = true;
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;defaultProductAbstraction doSell: {$this-&gt;_sold}&quot;.&#xB6;;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function doBuy() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;_bought = true;
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;defaultProductAbstraction doBuy: {$this-&gt;_bought}&quot;.&#xB6;;
&#xA0; &#xA0; }
}

class defaultProductImplementation extends defaultProductAbstraction {
&#xA0; &#xA0; public function doMore() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;defaultProductImplementation doMore()&quot;.&#xB6;;
&#xA0; &#xA0; }
}

class myProductImplementation extends defaultProductAbstraction {
&#xA0; &#xA0; public function doMore() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;myProductImplementation doMore() does more!&quot;.&#xB6;;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function doBuy() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;myProductImplementation&apos;s doBuy() and also my parent&apos;s dubai()&quot;.&#xB6;;
&#xA0; &#xA0; &#xA0; &#xA0; parent::doBuy();
&#xA0; &#xA0; }
}

class myProduct extends defaultProductImplementation {
&#xA0; &#xA0; private $_bought=true;
&#xA0; &#xA0; public function __construct() {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump($this-&gt;_bought);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function doBuy () {
&#xA0; &#xA0; &#xA0; &#xA0; /* non-default doBuy implementation */
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;_bought = true;
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;myProduct overrides the defaultProductImplementation&apos;s doBuy() here {$this-&gt;_bought}&quot;.&#xB6;;
&#xA0; &#xA0; }
}

class myOtherProduct extends myProductImplementation {
&#xA0; &#xA0; public function doBuy() {
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;myOtherProduct overrides myProductImplementations doBuy() here but still calls parent too&quot;.&#xB6;;
&#xA0; &#xA0; &#xA0; &#xA0; parent::doBuy();
&#xA0; &#xA0; }
}

echo &quot;new myProduct()&quot;.&#xB6;;
$product = new myProduct();

$product-&gt;doBuy();
$product-&gt;doSell();
$product-&gt;doMore();

echo &#xB6;.&quot;new defaultProductImplementation()&quot;.&#xB6;;

$newProduct = new defaultProductImplementation();
$newProduct-&gt;doBuy();
$newProduct-&gt;doSell();
$newProduct-&gt;doMore();

echo &#xB6;.&quot;new myProductImplementation&quot;.&#xB6;;
$lastProduct = new myProductImplementation();
$lastProduct-&gt;doBuy();
$lastProduct-&gt;doSell();
$lastProduct-&gt;doMore();

echo &#xB6;.&quot;new myOtherProduct&quot;.&#xB6;;
$anotherNewProduct = new myOtherProduct();
$anotherNewProduct-&gt;doBuy();
$anotherNewProduct-&gt;doSell();
$anotherNewProduct-&gt;doMore();
?>
```


Will result in:


```
<?php
/*
new myProduct()
bool(true)
myProduct overrides the defaultProductImplementation&apos;s doBuy() here 1
defaultProductAbstraction doSell: 1
defaultProductImplementation doMore()

new defaultProductImplementation()
defaultProductAbstraction doBuy: 1
defaultProductAbstraction doSell: 1
defaultProductImplementation doMore()

new myProductImplementation
myProductImplementation&apos;s doBuy() and also my parent&apos;s dubai()
defaultProductAbstraction doBuy: 1
defaultProductAbstraction doSell: 1
myProductImplementation doMore() does more!

new myOtherProduct
myOtherProduct overrides myProductImplementations doBuy() here but still calls parent too
myProductImplementation&apos;s doBuy() and also my parent&apos;s dubai()
defaultProductAbstraction doBuy: 1
defaultProductAbstraction doSell: 1
myProductImplementation doMore() does more!

*/
?>
```



  

#



Ok...the docs are a bit vague when it comes to an abstract class extending another abstract class.&#xA0; An abstract class that extends another abstract class doesn&apos;t need to define the abstract methods from the parent class.&#xA0; In other words, this causes an error:





```
<?php

abstract class class1 {

&#xA0; abstract public function someFunc();

}

abstract class class2 extends class1 {

&#xA0; abstract public function someFunc();

}

?>
```




Error: Fatal error: Can&apos;t inherit abstract function class1::someFunc() (previously declared abstract in class2) in /home/sneakyimp/public/chump.php on line 7



However this does not:





```
<?php

abstract class class1 {

&#xA0; abstract public function someFunc();

}

abstract class class2 extends class1 {

}

?>
```




An abstract class that extends an abstract class can pass the buck to its child classes when it comes to implementing the abstract methods of its parent abstract class.

  

#



Incidentally, abstract classes do not need to be base classes:



```
<?php
class Foo {
&#xA0; &#xA0; public function sneeze() { echo &apos;achoooo&apos;; }
}

abstract class Bar extends Foo {
&#xA0; &#xA0; public abstract function hiccup();
}

class Baz extends Bar {
&#xA0; &#xA0; public function hiccup() { echo &apos;hiccup!&apos;; }
}

$baz = new Baz();
$baz-&gt;sneeze();
$baz-&gt;hiccup();
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.abstract.php)

**[To root](/README.md)**