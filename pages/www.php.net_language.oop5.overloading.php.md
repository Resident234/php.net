# Overloading





This is a misuse of the term overloading. This article should call this technique &quot;interpreter hooks&quot;.

  

#



A word of warning!&#xA0; It may seem obvious, but remember, when deciding whether to use __get, __set, and __call as a way to access the data in your class (as opposed to hard-coding getters and setters), keep in mind that this will prevent any sort of autocomplete, highlighting, or documentation that your ide mite do.

Furthermore, it beyond personal preference when working with other people.&#xA0; Even without an ide, it can be much easier to go through and look at hardcoded member and method definitions in code, than having to sift through code and piece together the method/member names that are assembled in __get and __set.

If you still decide to use __get and __set for everything in your class, be sure to include detailed comments and documenting, so that the people you are working with (or the people who inherit the code from you at a later date) don&apos;t have to waste time interpreting your code just to be able to use it.

  

#



First off all, if you read this, please upvote the first comment on this list that states that &#x201C;overloading&#x201D; is a bad term for this behaviour. Because it REALLY is a bad name. You&#x2019;re giving new definition to an already accepted IT-branch terminology.

Second, I concur with all criticism you will read about this functionality. Just as naming it &#x201C;overloading&#x201D;, the functionality is also very bad practice. Please don&#x2019;t use this in a production environment. To be honest, avoid to use it at all. Especially if you are a beginner at PHP. It can make your code react very unexpectedly. In which case you MIGHT be learning invalid coding!

And last, because of __get, __set and __call the following code executes. Which is abnormal behaviour. And can cause a lot of problems/bugs.



```
<?php

class BadPractice {
&#xA0; // Two real properties
&#xA0; public $DontAllowVariableNameWithTypos = true;
&#xA0; protected $Number = 0;
&#xA0; // One private method
&#xA0; private function veryPrivateMethod() { }
&#xA0; // And three very magic methods that will make everything look inconsistent
&#xA0; // with all you have ever learned about PHP.
&#xA0; public function __get($n) {}
&#xA0; public function __set($n, $v) {}
&#xA0; public function __call($n, $v) {}
}

// Let&apos;s see our BadPractice in a production environment!
$UnexpectedBehaviour = new BadPractice;

// No syntax highlighting on most IDE&apos;s
$UnexpectedBehaviour-&gt;SynTaxHighlighting = false;

// No autocompletion on most IDE&apos;s
$UnexpectedBehaviour-&gt;AutoCompletion = false;

// Which will lead to problems waiting to happen
$UnexpectedBehaviour-&gt;DontAllowVariableNameWithTyphos = false; // see if below

// Get, Set and Call anything you want!
$UnexpectedBehaviour-&gt;EveryPosibleMethodCallAllowed(true, &apos;Why Not?&apos;);

// And sure, why not use the most illegal property names you can think off
$UnexpectedBehaviour-&gt;{&apos;100%Illegal+Names&apos;} = &apos;allowed&apos;;

// This Very confusing syntax seems to allow access to $Number but because of
// the lowered visibility it goes to __set()
$UnexpectedBehaviour-&gt;Number = 10;

// We can SEEM to increment it too! (that&apos;s really dynamic! :-) NULL++ LMAO
$UnexpectedBehaviour-&gt;Number++;

// this ofcourse outputs NULL (through __get) and not the PERHAPS expected 11
var_dump($UnexpectedBehaviour-&gt;Number);

// and sure, private method calls LOOK valid now!
// (this goes to __call, so no fatal error)
$UnexpectedBehaviour-&gt;veryPrivateMethod();

// Because the previous was __set to false, next expression is true
// if we didn&apos;t had __set, the previous assignment would have failed
// then you would have corrected the typho and this code will not have
// been executed. (This can really be a BIG PAIN)
if ($UnexpectedBehaviour-&gt;DontAllowVariableNameWithTypos) {
&#xA0; // if this code block would have deleted a file, or do a deletion on
&#xA0; // a database, you could really be VERY SAD for a long time!
&#xA0; $UnexpectedBehaviour-&gt;executeStuffYouDontWantHere(true);
}
?>
```



  

#



It is important to understand that encapsulation can be very easily violated in PHP. for example :
class Object{

}

$Object = new Object();
$Objet-&gt;barbarianProperties&#xA0; = &apos;boom&apos;;

var_dump($Objet);// object(Objet)#1 (1) { [&quot;barbarianProperties&quot;]=&gt; string(7) &quot;boom&quot; }

Hence it is possible to add a propertie out form the class definition.
It is then a necessity in order to protect encapsulation to introduce __set() in the class :

class Objet{
&#xA0; &#xA0; public function __set($name,$value){
&#xA0; &#xA0; &#xA0; &#xA0; throw new Exception (&apos;no&apos;);
&#xA0; &#xA0; }
}

  

#



Small vocabulary note: This is *not* &quot;overloading&quot;, this is &quot;overriding&quot;.

Overloading: Declaring a function multiple times with a different set of parameters like this:


```
<?php

function foo($a) {
&#xA0; &#xA0; return $a;
}

function foo($a, $b) {
&#xA0; &#xA0; return $a + $b;
}

echo foo(5); // Prints &quot;5&quot;
echo foo(5, 2); // Prints &quot;7&quot;

?>
```


Overriding: Replacing the parent class&apos;s method(s) with a new method by redeclaring it like this:


```
<?php

class foo {
&#xA0; &#xA0; function new($args) {
&#xA0; &#xA0; &#xA0; &#xA0; // Do something.
&#xA0; &#xA0; }
}

class bar extends foo {
&#xA0; &#xA0; function new($args) {
&#xA0; &#xA0; &#xA0; &#xA0; // Do something different.
&#xA0; &#xA0; }
}

?>
```



  

#



Using magic methods, especially __get(), __set(), and __call() will effectively disable autocomplete in most IDEs (eg.: IntelliSense) for the affected classes.

To overcome this inconvenience, use phpDoc to let the IDE know about these magic methods and properties: @method, @property, @property-read, @property-write.

/**
 * @property-read name
 * @property-read price
 */
class MyClass
{
&#xA0; &#xA0; private $properties = array(&apos;name&apos; =&gt; &apos;IceFruit&apos;, &apos;price&apos; =&gt; 2.49)
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __get($name)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;properties($name);
&#xA0; &#xA0; }
}

  

#



Be extra careful when using __call():&#xA0; if you typo a function call somewhere it won&apos;t trigger an undefined function error, but get passed to __call() instead, possibly causing all sorts of bizarre side effects.
In versions before 5.3 without __callStatic, static calls to nonexistent functions also fall through to __call!
This caused me hours of confusion, hopefully this comment will save someone else from the same.

  

#



If you want to make it work more naturally for arrays $obj-&gt;variable[] etc you&apos;ll need to return __get by reference.





```
<?php

class Variables

{

&#xA0; &#xA0; &#xA0; &#xA0; public function __construct()

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(session_id() === &quot;&quot;)

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; session_start();

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function __set($name,$value)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $_SESSION[&quot;Variables&quot;][$name] = $value;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function &amp;__get($name)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $_SESSION[&quot;Variables&quot;][$name];

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; public function __isset($name)

&#xA0; &#xA0; &#xA0; &#xA0; {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return isset($_SESSION[&quot;Variables&quot;][$name]);

&#xA0; &#xA0; &#xA0; &#xA0; }

}

?>
```



  

#



Example of usage __call() to have implicit getters and setters



```
<?php
class Entity {
&#xA0; &#xA0; public function __call($methodName, $args) {
&#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(&apos;~^(set|get)([A-Z])(.*)$~&apos;, $methodName, $matches)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $property = strtolower($matches[2]) . $matches[3];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!property_exists($this, $property)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new MemberAccessException(&apos;Property &apos; . $property . &apos; not exists&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; switch($matches[1]) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case &apos;set&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;checkArguments($args, 1, 1, $methodName);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;set($property, $args[0]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case &apos;get&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;checkArguments($args, 0, 0, $methodName);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;get($property);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case &apos;default&apos;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new MemberAccessException(&apos;Method &apos; . $methodName . &apos; not exists&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public function get($property) {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;$property;
&#xA0; &#xA0; }

&#xA0; &#xA0; public function set($property, $value) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;$property = $value;
&#xA0; &#xA0; &#xA0; &#xA0; return $this;
&#xA0; &#xA0; }

&#xA0; &#xA0; protected function checkArguments(array $args, $min, $max, $methodName) {
&#xA0; &#xA0; &#xA0; &#xA0; $argc = count($args);
&#xA0; &#xA0; &#xA0; &#xA0; if ($argc &lt; $min || $argc &gt; $max) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new MemberAccessException(&apos;Method &apos; . $methodName . &apos; needs minimaly &apos; . $min . &apos; and maximaly &apos; . $max . &apos; arguments. &apos; . $argc . &apos; arguments given.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

class MemberAccessException extends Exception{}

class Foo extends Entity {
&#xA0; &#xA0; protected $a;
}

$foo = new Foo();
$foo-&gt;setA(&apos;some&apos;); // outputs some
echo $foo-&gt;getA();

class Bar extends Entity {
&#xA0; &#xA0; protected $a;
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Custom setter.
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function setA($a) {
&#xA0; &#xA0; &#xA0; &#xA0; if (!preg_match(&apos;~^[0-9a-z]+$~i&apos;, $a)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new MemberAccessException(&apos;A can be only alphanumerical&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;a = $a;
&#xA0; &#xA0; &#xA0; &#xA0; return $this;
&#xA0; &#xA0; }
}

$bar = new Bar();
$bar-&gt;setA(&apos;abc123&apos;); // ok
$bar-&gt;setA(&apos;[]/*@...&apos;); // throws exception
?>
```



  

#



PHP 5.2.1

Its possible to call magic methods with invalid names using variable method/property names:



```
<?php

class foo
{
&#xA0; &#xA0; function __get($n)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; print_r($n);
&#xA0; &#xA0; }
&#xA0; &#xA0; function __call($m, $a)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; print_r($m);
&#xA0; &#xA0; }
}

$test = new foo;
$varname = &apos;invalid,variable+name&apos;;
$test-&gt;$varname;
$test-&gt;$varname();

?>
```


I just don&apos;t know if it is a bug or a feature :)

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.overloading.php)

**[To root](/README.md)**