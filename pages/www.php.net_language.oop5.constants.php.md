# Class Constants



it&apos;s possible to declare constant in base class, and override it in child, and access to correct value of the const from the static method is possible by &apos;get_called_class&apos; method:<br>

```
<?php
abstract class dbObject
{    
    const TABLE_NAME='undefined';
    
    public static function GetAll()
    {
        $c = get_called_class();
        return "SELECT * FROM `".$c::TABLE_NAME."`";
    }    
}

class dbPerson extends dbObject
{
    const TABLE_NAME='persons';
}

class dbAdmin extends dbPerson
{
    const TABLE_NAME='admins';
}

echo dbPerson::GetAll()."<br>";//output: "SELECT * FROM `persons`"
echo dbAdmin::GetAll()."<br>";//output: "SELECT * FROM `admins`"

?>
```
  

---

As of PHP 5.6 you can finally define constant using math expressions, like this one:<br><br>

```
<?php

class MyTimer {
    const SEC_PER_DAY = 60 * 60 * 24;
}

?>
```
<br><br>Me happy :)  

---

Most people miss the point in declaring constants and confuse then things by trying to declare things like functions or arrays as constants. What happens next is to try things that are more complicated then necessary and sometimes lead to bad coding practices. Let me explain...<br><br>A constant is a name for a value (but it&apos;s NOT a variable), that usually will be replaced in the code while it gets COMPILED and NOT at runtime. <br><br>So returned values from functions can&apos;t be used, because they will return a value only at runtime. <br><br>Arrays can&apos;t be used, because they are data structures that exist at runtime. <br><br>One main purpose of declaring a constant is usually using a value in your code, that you can replace easily in one place without looking for all the occurences. Another is, to avoid mistakes. <br><br>Think about some examples written by some before me: <br><br>1. const MY_ARR = "return array(\"A\", \"B\", \"C\", \"D\");";<br>It was said, this would declare an array that can be used with eval. WRONG! This is just a string as constant, NOT an array. Does it make sense if it would be possible to declare an array as constant? Probably not. Instead declare the values of the array as constants and make an array variable. <br><br>2. const magic_quotes = (bool)get_magic_quotes_gpc();<br>This can&apos;t work, of course. And it doesn&apos;t make sense either. The function already returns the value, there is no purpose in declaring a constant for the same thing. <br><br>3. Someone spoke about "dynamic" assignments to constants. What? There are no dynamic assignments to constants, runtime assignments work _only_ with variables. Let&apos;s take the proposed example: <br><br>

```
<?php
/**
 * Constants that deal only with the database
 */
class DbConstant extends aClassConstant {
    protected $host = 'localhost';
    protected $user = 'user';
    protected $password = 'pass';
    protected $database = 'db';
    protected $time;
    function __construct() {
        $this->time = time() + 1; // dynamic assignment
    }
}
?>
```
<br><br>Those aren&apos;t constants, those are properties of the class. Something like "this-&gt;time = time()" would even totally defy the purpose of a constant. Constants are supposed to be just that, constant values, on every execution. They are not supposed to change every time a script runs or a class is instantiated. <br><br>Conclusion: Don&apos;t try to reinvent constants as variables. If constants don&apos;t work, just use variables. Then you don&apos;t need to reinvent methods to achieve things for what is already there.  

---

const can also be used directly in namespaces, a feature never explicitly stated in the documentation.<br><br>

```
<?php
# foo.php
namespace Foo;

const BAR = 1;
?>
```




```
<?php
# bar.php
require 'foo.php';

var_dump(Foo\BAR); // => int(1)
?>
```
  

---

I think it&apos;s useful if we draw some attention to late static binding here:<br>

```
<?php
class A {
    const MY_CONST = false;
    public function my_const_self() {
        return self::MY_CONST;
    } 
    public function my_const_static() {
        return static::MY_CONST;
    } 
}

class B extends A {
   const MY_CONST = true;
}

$b = new B();
echo $b->my_const_self ? 'yes' : 'no'; // output: no
echo $b->my_const_static ? 'yes' : 'no'; // output: yes
?>
```
  

---

Hi, i would like to point out difference between self::CONST and $this::CONST with extended class.<br>Let us have class a:<br><br>

```
<?php 
class a {    
    const CONST_INT = 10;
    
    public function getSelf(){
        return self::CONST_INT;
    }
    
    public function getThis(){
        return $this::CONST_INT;
    }
}
?>
```


And class b (which extends a)



```
<?php
class b extends a {
    const CONST_INT = 20;
    
    public function getSelf(){
        return parent::getSelf();
    }
    
    public function getThis(){
        return parent::getThis();
    }
}
?>
```


Both classes have same named constant CONST_INT.
When child call method in parent class, there is different output between self and $this usage.



```
<?php
$b = new b();

print_r($b->getSelf());     //10
print_r($b->getThis());     //20

?>
```
  

---

[Editor&apos;s note: that is already possible as of PHP 5.6.0.]<br><br>Note, as of PHP7 it is possible to define class constants with an array.<br><br>

```
<?php
class MyClass
{
    const ABC = array('A', 'B', 'C');
    const A = '1';
    const B = '2';
    const C = '3';
    const NUMBERS = array(
        self::A,
        self::B,
        self::C,
    );
}
var_dump(MyClass::ABC);
var_dump(MyClass::NUMBERS);

// Result:
/*
array(3) {
    [0]=>
  string(1) "A"
    [1]=>
  string(1) "B"
    [2]=>
  string(1) "C"
}
array(3) {
    [0]=>
  string(1) "1"
    [1]=>
  string(1) "2"
    [2]=>
  string(1) "3"
}
*/
?>
```
  

---

Use CONST to set UPPER and LOWER LIMITS<br><br>If you have code that accepts user input or you just need to make sure input is acceptable, you can use constants to set upper and lower limits. Note: a static function that enforces your limits is highly recommended... sniff the clamp() function below for a taste.<br><br>

```
<?php

class Dimension
{
  const MIN = 0, MAX = 800;

  public $width, $height;

  public function __construct($w = 0, $h = 0){
    $this->width  = self::clamp($w);
    $this->height = self::clamp($h);
  }

  public function __toString(){
    return "Dimension [width=$this->width, height=$this->height]";
  }

  protected static function clamp($value){
    if($value < self::MIN) $value = self::MIN;
    if($value > self::MAX) $value = self::MAX;
    return $value;
  }
}

echo (new Dimension()) . '<br>';
echo (new Dimension(1500, 97)) . '<br>';
echo (new Dimension(14, -20)) . '<br>';
echo (new Dimension(240, 80)) . '<br>';

?>
```
<br><br>- - - - - - - -<br> Dimension [width=0, height=0] - default size<br> Dimension [width=800, height=97] - width has been clamped to MAX<br> Dimension [width=14, height=0] - height has been clamped to MIN<br> Dimension [width=240, height=80] - width and height unchanged<br>- - - - - - - -<br><br>Setting upper and lower limits on your classes also help your objects make sense. For example, it is not possible for the width or height of a Dimension to be negative. It is up to you to keep phoney input from corrupting your objects, and to avoid potential errors and exceptions in other parts of your code.  

---

[Official documentation page](https://www.php.net/manual/en/language.oop5.constants.php)

**[To root](/README.md)**