# Scope Resolution Operator (::)



A class constant, class property (static), and class function (static) can all share the same name and be accessed using the double-colon.<br><br>

```
<?php

class A {

    public static $B = '1'; # Static class variable.

    const B = '2'; # Class constant.
    
    public static function B() { # Static class function.
        return '3';
    }
    
}

echo A::$B . A::B . A::B(); # Outputs: 123
?>
```
  

#

In PHP, you use the self keyword to access static properties and methods.<br><br>The problem is that you can replace $this-&gt;method() with self::method() anywhere, regardless if method() is declared static or not. So which one should you use?<br><br>Consider this code:<br><br>class ParentClass {<br>    function test() {<br>        self::who();    // will output &apos;parent&apos;<br>        $this-&gt;who();    // will output &apos;child&apos;<br>    }<br><br>    function who() {<br>        echo &apos;parent&apos;;<br>    }<br>}<br><br>class ChildClass extends ParentClass {<br>    function who() {<br>        echo &apos;child&apos;;<br>    }<br>}<br><br>$obj = new ChildClass();<br>$obj-&gt;test();<br>In this example, self::who() will always output &#x2018;parent&#x2019;, while $this-&gt;who() will depend on what class the object has.<br><br>Now we can see that self refers to the class in which it is called, while $this refers to the class of the current object.<br><br>So, you should use self only when $this is not available, or when you don&#x2019;t want to allow descendant classes to overwrite the current method.  

#

It seems as though you can use more than the class name to reference the static variables, constants, and static functions of a class definition from outside that class using the :: . The language appears to allow you to use the object itself. <br><br>For example:<br>class horse <br>{<br>   static $props = {&apos;order&apos;=&gt;&apos;mammal&apos;};<br>}<br>$animal = new horse();<br>echo $animal::$props[&apos;order&apos;];<br><br>// yields &apos;mammal&apos;<br><br>This does not appear to be documented but I see it as an important convenience in the language. I would like to see it documented and supported as valid. <br><br>If it weren&apos;t supported officially, the alternative would seem to be messy, something like this:<br><br>$animalClass = get_class($animal);<br>echo $animalClass::$props[&apos;order&apos;];  

#

Just found out that using the class name may also work to call similar function of anchestor class.<br><br>

```
<?php

class Anchestor {
   
   public $Prefix = '';

   private $_string =  'Bar';
    public function Foo() {
        return $this->Prefix.$this->_string;
    }
}

class MyParent extends Anchestor {
    public function Foo() {
         $this->Prefix = null;
        return parent::Foo().'Baz';
    }
}

class Child extends MyParent {
    public function Foo() {
        $this->Prefix = 'Foo';
        return Anchestor::Foo();
    }
}

$c = new Child();
echo $c->Foo(); //return FooBar, because Prefix, as in Anchestor::Foo()

?>
```
<br><br>The Child class calls at Anchestor::Foo(), and therefore MyParent::Foo() is never run.  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.paamayim-nekudotayim.php)

**[To root](/README.md)**