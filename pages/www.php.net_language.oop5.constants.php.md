# Class Constants





it&apos;s possible to declare constant in base class, and override it in child, and access to correct value of the const from the static method is possible by &apos;get_called_class&apos; method:


```
<?php
abstract class dbObject
{&#xA0; &#xA0; 
&#xA0; &#xA0; const TABLE_NAME=&apos;undefined&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; public static function GetAll()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $c = get_called_class();
&#xA0; &#xA0; &#xA0; &#xA0; return &quot;SELECT * FROM `&quot;.$c::TABLE_NAME.&quot;`&quot;;
&#xA0; &#xA0; }&#xA0; &#xA0; 
}

class dbPerson extends dbObject
{
&#xA0; &#xA0; const TABLE_NAME=&apos;persons&apos;;
}

class dbAdmin extends dbPerson
{
&#xA0; &#xA0; const TABLE_NAME=&apos;admins&apos;;
}

echo dbPerson::GetAll().&quot;&lt;br&gt;&quot;;//output: &quot;SELECT * FROM `persons`&quot;
echo dbAdmin::GetAll().&quot;&lt;br&gt;&quot;;//output: &quot;SELECT * FROM `admins`&quot;

?>
```



  

#



As of PHP 5.6 you can finally define constant using math expressions, like this one:



```
<?php

class MyTimer {
&#xA0; &#xA0; const SEC_PER_DAY = 60 * 60 * 24;
}

?>
```


Me happy :)

  

#



Most people miss the point in declaring constants and confuse then things by trying to declare things like functions or arrays as constants. What happens next is to try things that are more complicated then necessary and sometimes lead to bad coding practices. Let me explain...



A constant is a name for a value (but it&apos;s NOT a variable), that usually will be replaced in the code while it gets COMPILED and NOT at runtime. 



So returned values from functions can&apos;t be used, because they will return a value only at runtime. 



Arrays can&apos;t be used, because they are data structures that exist at runtime. 



One main purpose of declaring a constant is usually using a value in your code, that you can replace easily in one place without looking for all the occurences. Another is, to avoid mistakes. 



Think about some examples written by some before me: 



1. const MY_ARR = &quot;return array(\&quot;A\&quot;, \&quot;B\&quot;, \&quot;C\&quot;, \&quot;D\&quot;);&quot;;

It was said, this would declare an array that can be used with eval. WRONG! This is just a string as constant, NOT an array. Does it make sense if it would be possible to declare an array as constant? Probably not. Instead declare the values of the array as constants and make an array variable. 



2. const magic_quotes = (bool)get_magic_quotes_gpc();

This can&apos;t work, of course. And it doesn&apos;t make sense either. The function already returns the value, there is no purpose in declaring a constant for the same thing. 



3. Someone spoke about &quot;dynamic&quot; assignments to constants. What? There are no dynamic assignments to constants, runtime assignments work _only_ with variables. Let&apos;s take the proposed example: 





```
<?php

/**

 * Constants that deal only with the database

 */

class DbConstant extends aClassConstant {

&#xA0; &#xA0; protected $host = &apos;localhost&apos;;

&#xA0; &#xA0; protected $user = &apos;user&apos;;

&#xA0; &#xA0; protected $password = &apos;pass&apos;;

&#xA0; &#xA0; protected $database = &apos;db&apos;;

&#xA0; &#xA0; protected $time;

&#xA0; &#xA0; function __construct() {

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;time = time() + 1; // dynamic assignment

&#xA0; &#xA0; }

}

?>
```




Those aren&apos;t constants, those are properties of the class. Something like &quot;this-&gt;time = time()&quot; would even totally defy the purpose of a constant. Constants are supposed to be just that, constant values, on every execution. They are not supposed to change every time a script runs or a class is instantiated. 



Conclusion: Don&apos;t try to reinvent constants as variables. If constants don&apos;t work, just use variables. Then you don&apos;t need to reinvent methods to achieve things for what is already there.

  

#



const can also be used directly in namespaces, a feature never explicitly stated in the documentation.



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
require &apos;foo.php&apos;;

var_dump(Foo\BAR); // =&gt; int(1)
?>
```



  

#



I think it&apos;s useful if we draw some attention to late static binding here:


```
<?php
class A {
&#xA0; &#xA0; const MY_CONST = false;
&#xA0; &#xA0; public function my_const_self() {
&#xA0; &#xA0; &#xA0; &#xA0; return self::MY_CONST;
&#xA0; &#xA0; } 
&#xA0; &#xA0; public function my_const_static() {
&#xA0; &#xA0; &#xA0; &#xA0; return static::MY_CONST;
&#xA0; &#xA0; } 
}

class B extends A {
&#xA0;&#xA0; const MY_CONST = true;
}

$b = new B();
echo $b-&gt;my_const_self ? &apos;yes&apos; : &apos;no&apos;; // output: no
echo $b-&gt;my_const_static ? &apos;yes&apos; : &apos;no&apos;; // output: yes
?>
```



  

#



Hi, i would like to point out difference between self::CONST and $this::CONST with extended class.
Let us have class a:



```
<?php 
class a {&#xA0; &#xA0; 
&#xA0; &#xA0; const CONST_INT = 10;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getSelf(){
&#xA0; &#xA0; &#xA0; &#xA0; return self::CONST_INT;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getThis(){
&#xA0; &#xA0; &#xA0; &#xA0; return $this::CONST_INT;
&#xA0; &#xA0; }
}
?>
```


And class b (which extends a)



```
<?php
class b extends a {
&#xA0; &#xA0; const CONST_INT = 20;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getSelf(){
&#xA0; &#xA0; &#xA0; &#xA0; return parent::getSelf();
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function getThis(){
&#xA0; &#xA0; &#xA0; &#xA0; return parent::getThis();
&#xA0; &#xA0; }
}
?>
```


Both classes have same named constant CONST_INT.
When child call method in parent class, there is different output between self and $this usage.



```
<?php
$b = new b();

print_r($b-&gt;getSelf());&#xA0; &#xA0;&#xA0; //10
print_r($b-&gt;getThis());&#xA0; &#xA0;&#xA0; //20

?>
```



  

#



[Editor&apos;s note: that is already possible as of PHP 5.6.0.]



Note, as of PHP7 it is possible to define class constants with an array.





```
<?php

class MyClass

{

&#xA0; &#xA0; const ABC = array(&apos;A&apos;, &apos;B&apos;, &apos;C&apos;);

&#xA0; &#xA0; const A = &apos;1&apos;;

&#xA0; &#xA0; const B = &apos;2&apos;;

&#xA0; &#xA0; const C = &apos;3&apos;;

&#xA0; &#xA0; const NUMBERS = array(

&#xA0; &#xA0; &#xA0; &#xA0; self::A,

&#xA0; &#xA0; &#xA0; &#xA0; self::B,

&#xA0; &#xA0; &#xA0; &#xA0; self::C,

&#xA0; &#xA0; );

}

var_dump(MyClass::ABC);

var_dump(MyClass::NUMBERS);



// Result:

/*

array(3) {

&#xA0; &#xA0; [0]=&gt;

&#xA0; string(1) &quot;A&quot;

&#xA0; &#xA0; [1]=&gt;

&#xA0; string(1) &quot;B&quot;

&#xA0; &#xA0; [2]=&gt;

&#xA0; string(1) &quot;C&quot;

}

array(3) {

&#xA0; &#xA0; [0]=&gt;

&#xA0; string(1) &quot;1&quot;

&#xA0; &#xA0; [1]=&gt;

&#xA0; string(1) &quot;2&quot;

&#xA0; &#xA0; [2]=&gt;

&#xA0; string(1) &quot;3&quot;

}

*/

?>
```



  

#



Use CONST to set UPPER and LOWER LIMITS

If you have code that accepts user input or you just need to make sure input is acceptable, you can use constants to set upper and lower limits. Note: a static function that enforces your limits is highly recommended... sniff the clamp() function below for a taste.



```
<?php

class Dimension
{
&#xA0; const MIN = 0, MAX = 800;

&#xA0; public $width, $height;

&#xA0; public function __construct($w = 0, $h = 0){
&#xA0; &#xA0; $this-&gt;width&#xA0; = self::clamp($w);
&#xA0; &#xA0; $this-&gt;height = self::clamp($h);
&#xA0; }

&#xA0; public function __toString(){
&#xA0; &#xA0; return &quot;Dimension [width=$this-&gt;width, height=$this-&gt;height]&quot;;
&#xA0; }

&#xA0; protected static function clamp($value){
&#xA0; &#xA0; if($value &lt; self::MIN) $value = self::MIN;
&#xA0; &#xA0; if($value &gt; self::MAX) $value = self::MAX;
&#xA0; &#xA0; return $value;
&#xA0; }
}

echo (new Dimension()) . &apos;&lt;br&gt;&apos;;
echo (new Dimension(1500, 97)) . &apos;&lt;br&gt;&apos;;
echo (new Dimension(14, -20)) . &apos;&lt;br&gt;&apos;;
echo (new Dimension(240, 80)) . &apos;&lt;br&gt;&apos;;

?>
```


- - - - - - - -
 Dimension [width=0, height=0] - default size
 Dimension [width=800, height=97] - width has been clamped to MAX
 Dimension [width=14, height=0] - height has been clamped to MIN
 Dimension [width=240, height=80] - width and height unchanged
- - - - - - - -

Setting upper and lower limits on your classes also help your objects make sense. For example, it is not possible for the width or height of a Dimension to be negative. It is up to you to keep phoney input from corrupting your objects, and to avoid potential errors and exceptions in other parts of your code.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.constants.php)

**[To root](/README.md)**