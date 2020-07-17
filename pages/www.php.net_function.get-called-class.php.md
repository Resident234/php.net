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

get_called_class() in closure-scopes:<br><br>

```
<?php
    ABSTRACT CLASS Base
    {
        protected static $stub = ['baz'];
        
        //final public function boot()
        static public function boot()
        {
            print __METHOD__.'-> '.get_called_class().PHP_EOL;
            
            array_walk(static::$stub, function()
            {
                print __METHOD__.'-> '.get_called_class().PHP_EOL;
            });
        }
        
        public function __construct()
        {
            self::boot();
            print __METHOD__.'-> '.get_called_class().PHP_EOL;
            
            array_walk(static::$stub, function()
            {
                print __METHOD__.'-> '.get_called_class().PHP_EOL;
            });
        }
    }
    
    CLASS Sub EXTENDS Base
    {
    }
    
    // static boot
        Base::boot(); print PHP_EOL;
            // Base::boot        -> Base
            // Base::{closure}    -> Base
            
        Sub::boot(); print PHP_EOL;
            // Base::boot        -> Sub
            // Base::{closure}    -> Base
            
        new sub;
            // Base::boot        -> Sub
            // Base::{closure}    -> Base
            // Base->__construct    -> Sub
            // Base->{closure}    -> Sub
    
    // instance boot
        new sub;
            // Base->boot        -> Sub
            // Base->{closure}    -> Sub
            // Base->__construct    -> Sub
            // Base->{closure}    -> Sub
?>
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