# get_called_class



As of PHP 5.5 you can also use "static::class" to get the name of the called class.<br><br>

```
<?php
class Bar {
    public static function test() {
        var_dump(static::class);
    }
}

class Foo extends Bar {

}

Foo::test();
Bar::test();
?>
```
<br><br>Output:<br><br>string(3) "Foo"<br>string(3) "Bar"  

#

SEE: http://php.net/manual/en/language.oop5.late-static-bindings.php<br><br>I think it is worth mentioning on this page, that many uses of the value returned by get_called_function() could be handled with the new use of the old keyword static, as in<br>

```
<?php 
static::$foo;
?>
```


versus


```
<?php
$that=get_called_class();
$that::$foo;
?>
```


I had been using $that:: as my conventional replacement for self:: until my googling landed me the url above.  I have replaced all uses of $that with static with success both as 


```
<?php 
static::$foo; //and...
new static();
?>
```
<br><br>Since static:: is listed with the limitation: "Another difference is that static:: can only refer to static properties." one may still need to use a $that:: to call static functions; though I have not yet needed this semantic.  

#

get_called_class() in closure-scopes:<br><br>&lt;?PHP<br>    ABSTRACT CLASS Base<br>    {<br>        protected static $stub = [&apos;baz&apos;];<br>        <br>        //final public function boot()<br>        static public function boot()<br>        {<br>            print __METHOD__.&apos;-&gt; &apos;.get_called_class().PHP_EOL;<br>            <br>            array_walk(static::$stub, function()<br>            {<br>                print __METHOD__.&apos;-&gt; &apos;.get_called_class().PHP_EOL;<br>            });<br>        }<br>        <br>        public function __construct()<br>        {<br>            self::boot();<br>            print __METHOD__.&apos;-&gt; &apos;.get_called_class().PHP_EOL;<br>            <br>            array_walk(static::$stub, function()<br>            {<br>                print __METHOD__.&apos;-&gt; &apos;.get_called_class().PHP_EOL;<br>            });<br>        }<br>    }<br>    <br>    CLASS Sub EXTENDS Base<br>    {<br>    }<br>    <br>    // static boot<br>        Base::boot(); print PHP_EOL;<br>            // Base::boot        -&gt; Base<br>            // Base::{closure}    -&gt; Base<br>            <br>        Sub::boot(); print PHP_EOL;<br>            // Base::boot        -&gt; Sub<br>            // Base::{closure}    -&gt; Base<br>            <br>        new sub;<br>            // Base::boot        -&gt; Sub<br>            // Base::{closure}    -&gt; Base<br>            // Base-&gt;__construct    -&gt; Sub<br>            // Base-&gt;{closure}    -&gt; Sub<br>    <br>    // instance boot<br>        new sub;<br>            // Base-&gt;boot        -&gt; Sub<br>            // Base-&gt;{closure}    -&gt; Sub<br>            // Base-&gt;__construct    -&gt; Sub<br>            // Base-&gt;{closure}    -&gt; Sub<br>?>
```
  

#

It is possible to write a completely self-contained Singleton base class in PHP 5.3 using get_called_class.<br><br>

```
<?php

abstract class Singleton {

    protected function __construct() {
    }

    final public static function getInstance() {
        static $aoInstance = array();

        $calledClassName = get_called_class();

        if (! isset ($aoInstance[$calledClassName])) {
            $aoInstance[$calledClassName] = new $calledClassName();
        }

        return $aoInstance[$calledClassName];
    }

    final private function __clone() {
    }
}

class DatabaseConnection extends Singleton {

    protected $connection;

    protected function __construct() {
        // @todo Connect to the database
    }

    public function __destruct() {
        // @todo Drop the connection to the database
    }
}

$oDbConn = new DatabaseConnection();  // Fatal error

$oDbConn = DatabaseConnection::getInstance();  // Returns single instance
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-called-class.php)

**[To root](/README.md)**