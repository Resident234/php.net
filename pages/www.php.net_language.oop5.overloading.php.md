# Overloading



This is a misuse of the term overloading. This article should call this technique "interpreter hooks".  

#

A word of warning!  It may seem obvious, but remember, when deciding whether to use __get, __set, and __call as a way to access the data in your class (as opposed to hard-coding getters and setters), keep in mind that this will prevent any sort of autocomplete, highlighting, or documentation that your ide mite do.<br><br>Furthermore, it beyond personal preference when working with other people.  Even without an ide, it can be much easier to go through and look at hardcoded member and method definitions in code, than having to sift through code and piece together the method/member names that are assembled in __get and __set.<br><br>If you still decide to use __get and __set for everything in your class, be sure to include detailed comments and documenting, so that the people you are working with (or the people who inherit the code from you at a later date) don&apos;t have to waste time interpreting your code just to be able to use it.  

#

First off all, if you read this, please upvote the first comment on this list that states that &#x201C;overloading&#x201D; is a bad term for this behaviour. Because it REALLY is a bad name. You&#x2019;re giving new definition to an already accepted IT-branch terminology.<br><br>Second, I concur with all criticism you will read about this functionality. Just as naming it &#x201C;overloading&#x201D;, the functionality is also very bad practice. Please don&#x2019;t use this in a production environment. To be honest, avoid to use it at all. Especially if you are a beginner at PHP. It can make your code react very unexpectedly. In which case you MIGHT be learning invalid coding!<br><br>And last, because of __get, __set and __call the following code executes. Which is abnormal behaviour. And can cause a lot of problems/bugs.<br><br>

```
<?php

class BadPractice {
  // Two real properties
  public $DontAllowVariableNameWithTypos = true;
  protected $Number = 0;
  // One private method
  private function veryPrivateMethod() { }
  // And three very magic methods that will make everything look inconsistent
  // with all you have ever learned about PHP.
  public function __get($n) {}
  public function __set($n, $v) {}
  public function __call($n, $v) {}
}

// Let's see our BadPractice in a production environment!
$UnexpectedBehaviour = new BadPractice;

// No syntax highlighting on most IDE's
$UnexpectedBehaviour->SynTaxHighlighting = false;

// No autocompletion on most IDE's
$UnexpectedBehaviour->AutoCompletion = false;

// Which will lead to problems waiting to happen
$UnexpectedBehaviour->DontAllowVariableNameWithTyphos = false; // see if below

// Get, Set and Call anything you want!
$UnexpectedBehaviour->EveryPosibleMethodCallAllowed(true, 'Why Not?');

// And sure, why not use the most illegal property names you can think off
$UnexpectedBehaviour->{'100%Illegal+Names'} = 'allowed';

// This Very confusing syntax seems to allow access to $Number but because of
// the lowered visibility it goes to __set()
$UnexpectedBehaviour->Number = 10;

// We can SEEM to increment it too! (that's really dynamic! :-) NULL++ LMAO
$UnexpectedBehaviour->Number++;

// this ofcourse outputs NULL (through __get) and not the PERHAPS expected 11
var_dump($UnexpectedBehaviour->Number);

// and sure, private method calls LOOK valid now!
// (this goes to __call, so no fatal error)
$UnexpectedBehaviour->veryPrivateMethod();

// Because the previous was __set to false, next expression is true
// if we didn't had __set, the previous assignment would have failed
// then you would have corrected the typho and this code will not have
// been executed. (This can really be a BIG PAIN)
if ($UnexpectedBehaviour->DontAllowVariableNameWithTypos) {
  // if this code block would have deleted a file, or do a deletion on
  // a database, you could really be VERY SAD for a long time!
  $UnexpectedBehaviour->executeStuffYouDontWantHere(true);
}
?>
```
  

#

It is important to understand that encapsulation can be very easily violated in PHP. for example :<br>class Object{<br><br>}<br><br>$Object = new Object();<br>$Objet-&gt;barbarianProperties  = &apos;boom&apos;;<br><br>var_dump($Objet);// object(Objet)#1 (1) { ["barbarianProperties"]=&gt; string(7) "boom" }<br><br>Hence it is possible to add a propertie out form the class definition.<br>It is then a necessity in order to protect encapsulation to introduce __set() in the class :<br><br>class Objet{<br>    public function __set($name,$value){<br>        throw new Exception (&apos;no&apos;);<br>    }<br>}  

#

Small vocabulary note: This is *not* "overloading", this is "overriding".<br><br>Overloading: Declaring a function multiple times with a different set of parameters like this:<br>

```
<?php

function foo($a) {
    return $a;
}

function foo($a, $b) {
    return $a + $b;
}

echo foo(5); // Prints "5"
echo foo(5, 2); // Prints "7"

?>
```


Overriding: Replacing the parent class's method(s) with a new method by redeclaring it like this:


```
<?php

class foo {
    function new($args) {
        // Do something.
    }
}

class bar extends foo {
    function new($args) {
        // Do something different.
    }
}

?>
```
  

#

Using magic methods, especially __get(), __set(), and __call() will effectively disable autocomplete in most IDEs (eg.: IntelliSense) for the affected classes.<br><br>To overcome this inconvenience, use phpDoc to let the IDE know about these magic methods and properties: @method, @property, @property-read, @property-write.<br><br>/**<br> * @property-read name<br> * @property-read price<br> */<br>class MyClass<br>{<br>    private $properties = array(&apos;name&apos; =&gt; &apos;IceFruit&apos;, &apos;price&apos; =&gt; 2.49)<br>    <br>    public function __get($name)<br>    {<br>        return $this-&gt;properties($name);<br>    }<br>}  

#

Be extra careful when using __call():  if you typo a function call somewhere it won&apos;t trigger an undefined function error, but get passed to __call() instead, possibly causing all sorts of bizarre side effects.<br>In versions before 5.3 without __callStatic, static calls to nonexistent functions also fall through to __call!<br>This caused me hours of confusion, hopefully this comment will save someone else from the same.  

#

If you want to make it work more naturally for arrays $obj-&gt;variable[] etc you&apos;ll need to return __get by reference.<br><br>

```
<?php
class Variables
{
        public function __construct()
        {
                if(session_id() === "")
                {
                        session_start();
                }
        }
        public function __set($name,$value)
        {
                $_SESSION["Variables"][$name] = $value;
        }
        public function &amp;__get($name)
        {
                return $_SESSION["Variables"][$name];
        }
        public function __isset($name)
        {
                return isset($_SESSION["Variables"][$name]);
        }
}
?>
```
  

#

Example of usage __call() to have implicit getters and setters<br><br>

```
<?php
class Entity {
    public function __call($methodName, $args) {
        if (preg_match('~^(set|get)([A-Z])(.*)$~', $methodName, $matches)) {
            $property = strtolower($matches[2]) . $matches[3];
            if (!property_exists($this, $property)) {
                throw new MemberAccessException('Property ' . $property . ' not exists');
            }
            switch($matches[1]) {
                case 'set':
                    $this->checkArguments($args, 1, 1, $methodName);
                    return $this->set($property, $args[0]);
                case 'get':
                    $this->checkArguments($args, 0, 0, $methodName);
                    return $this->get($property);
                case 'default':
                    throw new MemberAccessException('Method ' . $methodName . ' not exists');
            }
        }
    }

    public function get($property) {
        return $this->$property;
    }

    public function set($property, $value) {
        $this->$property = $value;
        return $this;
    }

    protected function checkArguments(array $args, $min, $max, $methodName) {
        $argc = count($args);
        if ($argc &lt; $min || $argc &gt; $max) {
            throw new MemberAccessException('Method ' . $methodName . ' needs minimaly ' . $min . ' and maximaly ' . $max . ' arguments. ' . $argc . ' arguments given.');
        }
    }
}

class MemberAccessException extends Exception{}

class Foo extends Entity {
    protected $a;
}

$foo = new Foo();
$foo->setA('some'); // outputs some
echo $foo->getA();

class Bar extends Entity {
    protected $a;
    /**
     * Custom setter.
     */
    public function setA($a) {
        if (!preg_match('~^[0-9a-z]+$~i', $a)) {
            throw new MemberAccessException('A can be only alphanumerical');
        }
        $this->a = $a;
        return $this;
    }
}

$bar = new Bar();
$bar->setA('abc123'); // ok
$bar->setA('[]/*@...'); // throws exception
?>
```
  

#

PHP 5.2.1<br><br>Its possible to call magic methods with invalid names using variable method/property names:<br><br>

```
<?php

class foo
{
    function __get($n)
    {
        print_r($n);
    }
    function __call($m, $a)
    {
        print_r($m);
    }
}

$test = new foo;
$varname = 'invalid,variable+name';
$test->$varname;
$test->$varname();

?>
```
<br><br>I just don&apos;t know if it is a bug or a feature :)  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.overloading.php)

**[To root](/README.md)**