# Visibility



INSIDE CODE and OUTSIDE CODE<br><br>

```
<?php

class Item
{
  /**
   * This is INSIDE CODE because it is written INSIDE the class.
   */
  public $label;
  public $price;
}

/**
 * This is OUTSIDE CODE because it is written OUTSIDE the class.
 */
$item = new Item();
$item->label = 'Ink-Jet Tatoo Gun';
$item->price = 49.99;

?>
```


Ok, that's simple enough... I got it inside and out. The big problem with this is that the Item class is COMPLETELY IGNORANT in the following ways:
* It REQUIRES OUTSIDE CODE to do all the work AND to know what and how to do it -- huge mistake.
* OUTSIDE CODE can cast Item properties to any other PHP types (booleans, integers, floats, strings, arrays, and objects etc.) -- another huge mistake.

Note: we did it correctly above, but what if someone made an array for $price? FYI: PHP has no clue what we mean by an Item, especially by the terms of our class definition above. To PHP, our Item is something with two properties (mutable in every way) and that's it. As far as PHP is concerned, we can pack the entire set of Britannica Encyclopedias into the price slot. When that happens, we no longer have what we expect an Item to be.

INSIDE CODE should keep the integrity of the object. For example, our class definition should keep $label a string and $price a float -- which means only strings can come IN and OUT of the class for label, and only floats can come IN and OUT of the class for price.



```
<?php

class Item
{
  /**
   * Here's the new INSIDE CODE and the Rules to follow:
   *
   * 1. STOP ACCESS to properties via $item->label and $item->price,
   *    by using the protected keyword.
   * 2. FORCE the use of public functions.
   * 3. ONLY strings are allowed IN &amp; OUT of this class for $label
   *    via the getLabel and setLabel functions.
   * 4. ONLY floats are allowed IN &amp; OUT of this class for $price
   *    via the getPrice and setPrice functions.
   */

  protected $label = 'Unknown Item'; // Rule 1 - protected.
  protected $price = 0.0;            // Rule 1 - protected.

  public function getLabel() {       // Rule 2 - public function.
    return $this->label;             // Rule 3 - string OUT for $label.
  }

  public function getPrice() {       // Rule 2 - public function.    
    return $this->price;             // Rule 4 - float OUT for $price.
  }

  public function setLabel($label)   // Rule 2 - public function.
  {
    /**
     * Make sure $label is a PHP string that can be used in a SORTING
     * alogorithm, NOT a boolean, number, array, or object that can't
     * properly sort -- AND to make sure that the getLabel() function
     * ALWAYS returns a genuine PHP string.
     *
     * Using a RegExp would improve this function, however, the main
     * point is the one made above.
     */

    if(is_string($label))
    {
      $this->label = (string)$label; // Rule 3 - string IN for $label.
    }
  }

  public function setPrice($price)   // Rule 2 - public function.
  {
    /**
     * Make sure $price is a PHP float so that it can be used in a
     * NUMERICAL CALCULATION. Do not accept boolean, string, array or
     * some other object that can't be included in a simple calculation.
     * This will ensure that the getPrice() function ALWAYS returns an
     * authentic, genuine, full-flavored PHP number and nothing but.
     *
     * Checking for positive values may improve this function,
     * however, the main point is the one made above.
     */

    if(is_numeric($price))
    {
      $this->price = (float)$price; // Rule 4 - float IN for $price.
    }
  }
}

?>
```
<br><br>Now there is nothing OUTSIDE CODE can do to obscure the INSIDES of an Item. In other words, every instance of Item will always look and behave like any other Item complete with a label and a price, AND you can group them together and they will interact without disruption. Even though there is room for improvement, the basics are there, and PHP will not hassle you... which means you can keep your hair!  

#

If you have problems with overriding private methods in extended classes, read this:)<br><br>The manual says that "Private limits visibility only to the class that defines the item". That means extended children classes do not see the private methods of parent class and vice versa also. <br><br>As a result, parents and children can have different implementations of the "same" private methods, depending on where you call them (e.g. parent or child class instance). Why? Because private methods are visible only for the class that defines them and the child class does not see the parent&apos;s private methods. If the child doesn&apos;t see the parent&apos;s private methods, the child can&apos;t override them. Scopes are different. In other words -- each class has a private set of private variables that no-one else has access to. <br><br>A sample demonstrating the percularities of private methods when extending classes:<br><br>

```
<?php
abstract class base {
    public function inherited() {
        $this->overridden();
    }
    private function overridden() {
        echo 'base';
    }
}

class child extends base {
    private function overridden() {
        echo 'child';
    }
}

$test = new child();
$test->inherited();
?>
```


Output will be "base".

If you want the inherited methods to use overridden functionality in extended classes but public sounds too loose, use protected. That's what it is for:)

A sample that works as intended:



```
<?php
abstract class base {
    public function inherited() {
        $this->overridden();
    }
    protected function overridden() {
        echo 'base';
    }
}

class child extends base {
    protected function overridden() {
        echo 'child';
    }
}

$test = new child();
$test->inherited();
?>
```
<br>Output will be "child".  

#

Just a quick note that it&apos;s possible to declare visibility for multiple properties at the same time, by separating them by commas.<br><br>eg:<br><br>

```
<?php
class a
{
    protected $a, $b;

    public $c, $d;

    private $e, $f;
}
?>
```
  

#

A class A static public function can access to class A private function :<br><br>

```
<?php
class A {
    private function foo()
    {
        print("bar");
    }

    static public function bar($a)
    {
        $a->foo();
    }
}

$a = new A();

A::bar($a);
?>
```
<br><br>It&apos;s working.  

#

Beware: Visibility works on a per-class-base and does not prevent instances of the same class accessing each others properties!<br><br>

```
<?php
class Foo
{
    private $bar;

    public function debugBar(Foo $object)
    {
        // this does NOT violate visibility although $bar is private
        echo $object->bar, "\n";
    }

    public function setBar($value)
    {
        // Neccessary method, for $bar is invisible outside the class
        $this->bar = $value;
    }
    
    public function setForeignBar(Foo $object, $value)
    {
        // this does NOT violate visibility!
        $object->bar = $value;
    }
}

$a = new Foo();
$b = new Foo();
$a->setBar(1);
$b->setBar(2);
$a->debugBar($b);        // 2
$b->debugBar($a);        // 1
$a->setForeignBar($b, 3);
$b->setForeignBar($a, 4);
$a->debugBar($b);        // 3
$b->debugBar($a);        // 4
?>
```
  

#

Please note that protected methods are also available from sibling classes as long as the method is declared in the common parent. This may also be an abstract method.<br> <br>In the below example Bar knows about the existence of _test() in Foo because they inherited this method from the same parent. It does not matter that it was abstract in the parent.<br><br>

```
<?php
abstract class Base {
    abstract protected function _test();
}
 
class Bar extends Base {
    
    protected function _test() { }
    
    public function TestFoo() {
        $c = new Foo();
        $c->_test();
    }
}
 
class Foo extends Base {
    protected function _test() {
        echo 'Foo';
    }
}
 
$bar = new Bar();
$bar->TestFoo(); // result: Foo
?>
```
  

#

This has already been noted here, but there was no clear example. Methods defined in a parent class can NOT access private methods defined in a class which inherits from them. They can access protected, though.<br><br>Example:<br><br>

```
<?php

class ParentClass {

    public function execute($method) {
        $this->$method();
    }
    
}

class ChildClass extends ParentClass {

    private function privateMethod() {
        echo "hi, i'm private";
    }
    
    protected function protectedMethod() {
        echo "hi, i'm protected";
    }
    
}

$object = new ChildClass();

$object->execute('protectedMethod');

$object->execute('privateMethod');

?>
```
<br><br>Output:<br><br>hi, i&apos;m protected<br>Fatal error: Call to private method ChildClass::privateMethod() from context &apos;ParentClass&apos; in index.php on line 6<br><br>In an early approach this may seem unwanted behaviour but it actually makes sense. Private can only be accessed by the class which defines, neither parent nor children classes.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.visibility.php)

**[To root](/README.md)**